---
description: Run the weekly roundup scan (or any vertical roundup once configured) — scan newsletters and the web, surface candidates, let the user pick, then stage the selection for drafting. Orchestrates the news-curator subagent. Research finds; Comms Desk writes — use `--draft` (requires comms) to chain straight into drafting, or run comms's `/post` manually later. Renamed from `/ai-roundup` (2026-09-15); `/ai-roundup` remains a deprecation alias.
---

# /roundup

Pipeline for finding and staging the week's roundup candidates.

This command orchestrates the news-curator subagent and a user-pick step. It's a slash command (not a subagent) so the user can intervene — pick which candidates to include, tweak the angle, rerun the scan. It stops once the picks are staged; drafting is comms's job (`/post`), not research's.

---

## Step 1 — Pre-flight

Read `<config-root>/plugins/research.user-context.md`. If missing or unpopulated, route to `/setup-news` and stop.

If populated, surface a one-line config check:
> "Configured for: **[topic]** for **[audience]**. Sources: [N preferred + open web]. Voice: [3 voice descriptors]. Length target: [short/medium/long]. Look right? (Y to proceed, edit to adjust)"

If the user says "edit," route to `/setup-news`. Otherwise proceed.

---

## Step 2 — Scan and rank (delegate to news-curator)

Use the Task tool with `subagent_type="news-curator"` and pass:
- `user-context-path` — the absolute path to `<config-root>/plugins/research.user-context.md`
- `time-window` — default 7 days, override if user specified
- `max-candidates` — default 10

Wait for the candidate list to return.

---

## Step 3 — Show the candidates and let the user pick

Render the candidate list cleanly:

```
**Top candidates this week:**

1. [Headline] — [score]/10
   [why it matters]
   [Source]

2. ...
```

Then ask:
> "Which to include? Reply with numbers (e.g., '1,2,4,6,8') or 'all top N' or 'pick for me' to let me choose the strongest 5–7."

If user says "pick for me" — select top 5–7 by score, biased toward variety (don't pick 6 stories from the same source or same sub-theme).

If user says specific numbers — use those.

If user wants to skip the selection step entirely (`/roundup --auto-pick`), use the top 5–7 by score automatically.

---

## Step 4 — Stage the picks and stop

Write the selected candidates to `<config-root>/staged/roundup/[YYYY-MM-DD].md` (today's date), containing:

```markdown
# Roundup candidates — [YYYY-MM-DD]

## Selected candidates
For each selected item: headline, one-sentence summary, why it matters, source link, relevance score.

## Themes of the week
[from news-curator's output]

## Notable tools / releases
[from news-curator's output, if any]

## Confidence & gaps
[from news-curator's output]
```

Confirm to the user:
> "Staged [N] candidates to `<config-root>/staged/roundup/[date].md`. Run comms's `/post` to draft the roundup in your voice — or `/roundup --draft` next time to chain straight into drafting."

Research's job ends here. **Do not draft a post** — that's comms's `/post` command (which uses the `post-assembler` agent). Research finds; Comms Desk writes.

### `--draft` flag — chain into comms

If invoked as `/roundup --draft`:

1. Check whether comms is installed: look for `<config-root>/plugins/comms.user-context.md` (comms configured) or otherwise detect the `comms` plugin/command set is available in this environment.
2. **If comms is available** — after writing the staged file, invoke comms's `/post` command directly, passing the staged file path as its input, instead of the confirmation message above. Let `/post` take over the rest of the interaction (draft, iterate, ship).
3. **If comms is not installed/available** — write the staged file as normal, then tell the user:
   > "Staged [N] candidates to `<config-root>/staged/roundup/[date].md`. Drafting isn't available — the Comms Desk plugin isn't installed. Install it and run `/post` to draft this roundup in your voice."
   Do not fail or error; `/roundup --draft` degrades to the same behavior as plain `/roundup` when comms is absent.

---

## Behavior rules

- **Don't skip Step 3** unless the user explicitly opts out. Picking is part of the user's editorial voice — even "pick for me" is an explicit opt-out, not a default.
- **This command doesn't draft.** Drafting lives in comms's `/post` (and its `post-assembler` agent). If a user asks `/roundup` to "just write the post," point them at `/post` or `/roundup --draft`.
- **Be honest about gaps.** If news-curator surfaced a slow week, tell the user upfront — "Slow week, only 6 strong candidates" — so they can choose to wait, broaden the topic, or stage anyway.
