# Changelog

All notable changes to news-curator are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.2.6] — Skill auto-invocation audit (2026-09-15)

Nucleus Operating Model Refactor Phase 3 step 3.7. Ritual and side-effecting
skills marked `disable-model-invocation: true` so they only run on explicit
invocation, not loose natural-language matching — the model can still be
asked to run them by name. Read-mostly, low-stakes, or high-frequency
conversational skills are left auto-invocable. Marketplace-wide this brings
model-invocable skills from ~81 to 27, under the ≤30 target audited with
`/skill-doctor`.

### Changed
- Marked `disable-model-invocation: true` on: `ai-roundup`, `setup`, `setup-news`.

## [0.2.5] — Identity/voice moved to memory/me/ (2026-09-15)

### Changed
- Path references updated from `<config-root>/identity.md` / `<config-root>/voice.md` to `<config-root>/memory/me/identity.md` / `<config-root>/memory/me/voice.md`, per the Nucleus Operating Model Refactor Phase 2 scopes restructure (identity/voice are personal, not org-shared facts). No behavior change beyond the path.

## [0.2.4] — OpenAI host adapter (2026-09-14)

### Added
- Native Codex/ChatGPT plugin manifest, durable `AGENTS.md` entrypoint, and an explicit OpenAI capability/degradation contract.
- GPT-discoverable skill aliases for canonical command workflows and read-only Codex role bindings where this plugin ships agents.
- Shared config-root resolution compatible with Cortex and Claude; all GPT tests use repository fixtures or temporary directories only.

## [0.2.3] — Platform-agnostic Step 0 (2026-05-12)

### Changed
- **Setup command Step 0 now platform-agnostic.** Every `request_cowork_directory(...)` call is conditional: "In Cowork, call `request_cowork_directory(...)`. In Claude Code (or any environment with direct filesystem access), no mount is needed." Same plugin source works in both runtimes.

### Why this matters
Phase 0 of SECOND-BRAIN-V2-SPEC. Removes the implicit Cowork-only assumption so Claude Code users do not hit unsupported tool calls during setup.

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
