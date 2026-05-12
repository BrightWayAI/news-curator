# Changelog

All notable changes to news-curator are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.2.0] — Config-root refactor

### Changed
- **Plugin config moved to a user-chosen folder.** Reads/writes now go to `<config-root>/plugins/news-curator.user-context.md` via the pointer at `~/Documents/.claude-plugin-config-root`.
- **`/setup-news` Step 0 bootstraps the config root** and reads shared identity + voice.
- **`post-assembler` subagent** now reads voice from `<config-root>/voice.md` (the canonical voice file) — same path other drafting plugins use.
- **`/ai-roundup` updated** to read the new path.
- **User-facing prompts debranded** for fork-friendliness.

## [0.1.0] — Initial release

### Added
- Two subagents that handle the heavy lift end-to-end:
  - `news-curator` — scans newsletters and the open web for the past N days, ranks candidates for the user's audience, returns ~10 with summaries, scores, and source links.
  - `post-assembler` — takes selected candidates plus voice rules and drafts a LinkedIn-ready post (body + first-comment with sources + alternate hook for A/B). Reads `~/Documents/Claude/voice.md` (shared voice from cortex) as canonical voice; falls back to plugin user-context for plugin-specific overrides.
- `/ai-roundup` slash command orchestrates the full pipeline: scan → user picks candidates → draft → optional iteration → optional save to `runs/[date].md`.
- Setup interview captures topic, audience, sources, voice, banned phrases, post format, cadence, hashtag preferences.
- Configurable per topic — works for AI, climate, fintech, or any vertical.
- Hard caps to respect copyright (≤15-word verbatim quotes) and avoid paywall scraping (paywalled content from non-subscribed sources flagged and skipped).
