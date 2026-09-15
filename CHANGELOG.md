# Changelog

All notable changes to news-curator are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/). Versions match `plugin.json`.

## [0.5.0] — `/setup-voice` ownership moves to Comms Desk (2026-09-15)

### Changed
- `commands/setup-news.md`, `skills/setup/SKILL.md`, and `README.md` updated to attribute `/setup-voice` to `comms` instead of `cortex` — voice capture itself was already read-only for this plugin, so no behavior changed, only attribution.

## [0.4.0] — Research finds, Comms Desk writes (2026-09-15)

### Changed
- `/roundup` no longer drafts a post. It now stops after the user picks candidates, writing them to `<config-root>/staged/roundup/<date>.md` (summaries + links + themes). Drafting moved entirely to comms's new `/post` command.
- The `post-assembler` agent moved out of this plugin into `comms/agents/post-assembler.md` — research's remaining subagent is `news-curator` only.
- `/roundup` gained a `--draft` flag: when comms is installed, it chains straight from staging into comms's `/post`. When comms isn't installed, `/roundup --draft` degrades to plain `/roundup` behavior (stages, tells the user drafting isn't available, and points at `/post` once comms is installed) rather than failing.
- `/setup-news` no longer asks voice-descriptor / banned-phrase / sentence-length / hook-pattern interview questions. Voice is read directly from `<config-root>/memory/me/voice.md` (cortex's `/setup-voice`) at draft time by comms; this plugin's interview now covers topic, audience, sources, and roundup-specific format only.
- Plugin description updated to reflect find/curate-only scope; drafting ownership language removed.

### Coordination note
- `/setup-news` documents that voice capture is not this plugin's responsibility. If a parallel `comms:setup-voice`-style flow is introduced, this plugin doesn't need further changes — it never reads voice fields itself.

## [0.3.0] — Renamed to research (2026-09-15)

### Changed
- Renamed from `news-curator` to `research` (display name: Research) as part of the 2026-09-15 Nucleus plugin rename. Old plugin ID/repo name redirects; see marketplace catalog.
- `/ai-roundup` renamed to `/roundup`; `/ai-roundup` kept as a thin deprecation alias.
- Plugin config path updated to `<config-root>/plugins/research.user-context.md`; optional run archive path updated to `<config-root>/research/runs/`.

## [0.2.8] — host-neutral role metadata (2026-09-15)

### Changed
- Added host-neutral deep-reasoning intent alongside Claude model bindings for research and assembly roles.
- Standardized setup on the vendor-neutral config-root resolver and moved optional run archives out of the installed plugin directory.

## [0.2.7] — Codex adapter synchronization (2026-09-15)

### Fixed
- Synchronized the Codex manifest with the current plugin version.

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
