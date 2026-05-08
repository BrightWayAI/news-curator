# Changelog

All notable changes to news-curator are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.1.0] — Initial release

### Added
- Two subagents that handle the heavy lift end-to-end:
  - `news-curator` — scans newsletters and the open web for the past N days, ranks candidates for the user's audience, returns ~10 with summaries, scores, and source links.
  - `post-assembler` — takes selected candidates plus voice rules and drafts a LinkedIn-ready post (body + first-comment with sources + alternate hook for A/B). Reads `~/Documents/Claude/voice.md` (shared voice from cortex) as canonical voice; falls back to plugin user-context for plugin-specific overrides.
- `/ai-roundup` slash command orchestrates the full pipeline: scan → user picks candidates → draft → optional iteration → optional save to `runs/[date].md`.
- Setup interview captures topic, audience, sources, voice, banned phrases, post format, cadence, hashtag preferences.
- Configurable per topic — works for AI, climate, fintech, or any vertical.
- Hard caps to respect copyright (≤15-word verbatim quotes) and avoid paywall scraping (paywalled content from non-subscribed sources flagged and skipped).
