# news-curator

Curate and draft a weekly news roundup post — built originally for AI roundups on LinkedIn, but the topic and audience are configurable, so it works for any vertical (climate, infra, security, your industry).

Two subagents handle the heavy lift:

- **news-curator** — scans the past 7 days of newsletters and the open web, ranks stories for your audience, and returns ~10 candidates with summaries and source links.
- **post-assembler** — takes the candidates you pick + your voice rules and drafts the LinkedIn post (and a first-comment with sources).

A single slash command (`/ai-roundup`) orchestrates the whole pipeline.

## Install

Recommended: install via the [BrightWayAI marketplace](https://github.com/BrightWayAI/nucleus).

Or directly:

```
/plugin marketplace add BrightWayAI/news-curator
/plugin install news-curator@news-curator
```

## First-time setup

Run `/setup-news`. The setup skill walks you through a short interview and captures:

- **Topic and audience** — what vertical you cover (AI / climate / etc.) and who you're writing for (leaders, ICs, engineers, investors).
- **Sources** — newsletters, RSS feeds, and sites to prioritize.
- **Voice and format** — banned phrases, tone, post length, structure preferences (e.g., "5 bullets + first-comment with links").
- **Cadence** — when you typically post and how the candidate list should be sized.

Answers are saved to `references/user-context.md` (gitignored). Both agents read this before working.

You can re-run `/setup-news` anytime to update.

## What's inside

```
.claude-plugin/plugin.json     Plugin manifest
agents/
  news-curator.md              Subagent: scans + ranks stories
  post-assembler.md            Subagent: drafts the LinkedIn post
commands/
  ai-roundup.md                Slash command: full pipeline
  setup-news.md                Interview and config writer
skills/
  setup/SKILL.md               Auto-fires on setup phrases
  ai-roundup/SKILL.md          Auto-fires on roundup phrases
references/
  user-context.template.md     Structure (committed)
  user-context.md              Your config (gitignored, created by setup)
```

## Dependencies

- `WebSearch` — required for scanning the open web.
- A web-fetch tool (built-in) — for reading source articles.
- That's it. Optional: a Drive connector if you want to drop the candidate list to a file before drafting.

## License

MIT. See `LICENSE`.
