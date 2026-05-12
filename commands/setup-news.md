---
description: Configure news-curator for your topic, audience, sources, and voice via a short interview. Writes results to <config-root>/plugins/news-curator.user-context.md so news-curator and post-assembler can do real work. Re-run anytime to update.
---

# /setup-news

Short interview that captures the context news-curator and post-assembler need to be useful for *you* — your topic, your audience, your voice, your sources.

---

## Step 0 — Resolve plugin config root

Per-plugin config in this marketplace lives under a user-chosen folder, recorded at `~/Documents/.claude-plugin-config-root` (single-line text file in the user's home).

### A — Try the pointer

Ensure access to `~/Documents`. In Cowork, call `request_cowork_directory(~/Documents)` once if not already granted. In Claude Code (or any environment with direct filesystem access), no mount is needed. Then read `~/Documents/.claude-plugin-config-root`.
- **Exists**: read line 1 → that's the config root path. Ensure access to `<config-root>`. If running in Cowork and the folder isn't already mounted in this session, call `request_cowork_directory(<config-root>)`. If running in Claude Code or another environment with direct filesystem access, no mount call is needed. Skip to section C.
- **Missing**: continue to section B.

### B — First-time bootstrap

Prompt: "First-time plugin setup. Where should I store your plugin config — identity, voice, and per-plugin settings? Pick a folder you control (e.g., `~/Documents/Claude/` or `~/Documents/PluginConfig/`). The folder will hold `identity.md`, `voice.md`, and a `plugins/` subdirectory."

Then:
1. Ensure access to `<path>`. If running in Cowork and the folder isn't already mounted in this session, call `request_cowork_directory(<path>)`. If running in Claude Code or another environment with direct filesystem access, no mount call is needed. Create `<path>/plugins/`. Write absolute path to `~/Documents/.claude-plugin-config-root`.

### C — Read shared identity and voice

Read `<config-root>/identity.md` (cortex's `/setup-identity`) and `<config-root>/voice.md` (cortex's `/setup-voice`). The post-assembler subagent in this plugin reads voice directly from this file at draft time.

- **Identity populated** → use as background context (post-assembler addresses the user correctly).
- **Voice populated** → pre-fill Section 3 (Voice and format) of this interview. Skip those questions; just confirm.
- **Missing** → offer to run `/setup-identity` and/or `/setup-voice` first, or proceed inline.

For the rest of this document, **`<config-root>`** refers to the resolved path. This plugin's config file lives at **`<config-root>/plugins/news-curator.user-context.md`**.

---

## Step 1 — Check for existing config

Read `<config-root>/plugins/news-curator.user-context.md` if it exists.

- If populated → ask: "You've already configured news-curator. Update specific sections, or start over?"
  - "Update [section]" → jump to that section.
  - "Start over" → run full interview.
- If not → start fresh. Read `references/user-context.template.md` for structure.

---

## Step 2 — The interview

One section at a time. Confirm before moving to the next.

### Section 1 — Topic and audience

- What topic / vertical do you cover? (AI / climate / fintech / security / healthcare / [other])
- Who's your audience? Be specific — "leaders and operators," "ML engineers," "policy people," "founders," "investors." Knowing this drives the ranking.
- One sentence describing what you want this audience to walk away with after reading your weekly post.

### Section 2 — Sources

- Which newsletters do you actually read? (List 3–10 — Stratechery, Ben's Bites, Last Week in AI, etc.)
- Which sites do you trust for primary reporting? (e.g., The Information, Wired, Reuters)
- Anything you explicitly want to *avoid*? (e.g., "no Hacker News meta-discussions," "skip aggregators that just rehash other reporting")
- Are you subscribed to any paywalled outlets? (If yes, those become fair game for body summaries; if no, news-curator notes the headline and skips.)

### Section 3 — Voice and format

- Three words describing your voice. (e.g., "warm, direct, contrarian.")
- Banned phrases — words you don't use. (Common: "leverage," "synergy," "delight," "game-changer.")
- Sentence length preference — short and punchy / mixed / longer-form?
- Hook patterns you like — contrarian / observation / prediction / question / mix?
- Body format — bullets / numbered list / narrative paragraphs / mix?
- Where do source links go — in the post body, or in a first-comment? (Default: first-comment — better for LinkedIn distribution.)
- Sign-off / CTA — anything you say at the end of every roundup post? Or none?
- Hashtags — preferred set, max count, or "no hashtags"?

### Section 4 — Cadence and length

- What day/time do you typically post? (Friday afternoon? Monday morning?)
- Length target by default — short (≤700 chars) / medium (700–1500) / long (1500–3000)?
- How many candidates do you typically want surfaced? (Default 10. Some people prefer 7 to force harder cuts; some prefer 15 for more selection.)

---

## Step 3 — Write the config

Populate `<config-root>/plugins/news-curator.user-context.md` with answers, structured for fast agent reads:

```markdown
# news-curator user context

_Last updated: [date]_

## Topic and audience
- **Topic:** ...
- **Audience:** ...
- **Walkaway:** ...

## Sources
- **Preferred newsletters:** ...
- **Trusted sites:** ...
- **Avoid:** ...
- **Paywalled subscriptions:** ...

## Voice
- **Three words:** ...
- **Banned phrases:** ...
- **Sentence length:** ...
- **Hook patterns:** ...

## Format
- **Body format:** ...
- **Source links:** [in-comment / in-body]
- **Sign-off / CTA:** ...
- **Hashtags:** ...

## Cadence
- **Post day/time:** ...
- **Default length:** [short / medium / long]
- **Candidate count:** ...
```

---

## Step 4 — Confirm and offer next step

Summarize what was saved (one short paragraph) and offer:
> "Try `/ai-roundup` to run the full pipeline — scan, pick, draft. Should take 3–5 minutes."

---

## Behavior rules

- **One section at a time.** Don't blast all questions.
- **Skip what doesn't apply.** "I don't have banned phrases yet" is a valid answer — capture it as such.
- **Idempotent.** Re-running `/setup-news` updates without re-doing finished sections.
- **Keep it tight.** Whole interview should take under 5 minutes if the user has answers ready.
