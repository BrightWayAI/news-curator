---
description: Run the full weekly AI roundup pipeline (or any vertical roundup once configured) — scan newsletters and the web, surface candidates, let the user pick, then draft a LinkedIn-ready post in their voice. Orchestrates the news-curator and post-assembler subagents end-to-end.
---

# /ai-roundup

End-to-end pipeline for the weekly roundup post.

This command orchestrates two subagents and a user-pick step in the middle. It's a slash command (not a subagent) so the user can intervene between steps — pick which candidates to include, tweak the angle, regenerate.

---

## Step 1 — Pre-flight

Read `<config-root>/plugins/news-curator.user-context.md`. If missing or unpopulated, route to `/setup-news` and stop.

If populated, surface a one-line config check:
> "Configured for: **[topic]** for **[audience]**. Sources: [N preferred + open web]. Voice: [3 voice descriptors]. Length target: [short/medium/long]. Look right? (Y to proceed, edit to adjust)"

If the user says "edit," route to `/setup-news`. Otherwise proceed.

---

## Step 2 — Scan and rank (delegate to news-curator)

Use the Task tool with `subagent_type="news-curator"` and pass:
- `user-context-path` — the absolute path to `<config-root>/plugins/news-curator.user-context.md`
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

If user wants to skip the selection step entirely (`/ai-roundup --auto-pick`), use the top 5–7 by score automatically.

---

## Step 4 — Draft the post (delegate to post-assembler)

Use the Task tool with `subagent_type="post-assembler"` and pass:
- `user-context-path`
- `candidates` — the user's selected items (full data, not just numbers)
- `themes` — themes of the week from news-curator's output
- `hook-style` — default "auto," override if user specified
- `length-target` — from user-context, override if user specified

Wait for the draft to return.

---

## Step 5 — Show the draft and offer iteration

Render the draft:

```
**Draft:**

[full post]

**First comment (sources):**

[sources list]

**Alternate hook:**

[the alt hook]
```

Then offer:
> "What now? — 'ship it' (copy-paste ready) — 'use the alt hook' — 'shorter' / 'longer' — 'sharper hook' (regenerates with a different style) — 'redo with X' (your direction)"

For most edits, you can iterate inline without re-invoking post-assembler. For "redo with X" or major angle shifts, re-invoke post-assembler with updated guidance in the brief.

---

## Step 6 — Done

When the user says "ship it," do a final pass:
- Verify no banned phrases (per user-context)
- Verify length is within target
- Verify hashtags per user-context
- Output the final post + first-comment cleanly, ready to copy-paste.

Then ask:
> "Save this run to `runs/[date].md` for the archive? (Y/N)"

If yes, write to `runs/[YYYY-MM-DD].md` with: timestamp, candidates considered, candidates selected, final post, alternate hook. (`runs/` is gitignored — local archive only.)

---

## Behavior rules

- **Don't skip Step 3** unless the user explicitly opts out. Picking is part of the user's editorial voice — even "pick for me" is an explicit opt-out, not a default.
- **Iterate without re-scanning.** Steps 4–6 should be fast. Re-scanning the web is expensive; only do it if the user explicitly asks ("rerun the scan, the news shifted today").
- **Voice over speed.** If the draft doesn't sound like the user, fix it. A delayed post in their voice beats a fast post in generic LinkedIn-ese.
- **Be honest about gaps.** If news-curator surfaced a slow week, tell the user upfront — "Slow week, only 6 strong candidates" — so they can choose to wait, broaden the topic, or post a different format.
