# Security Policy

## What this plugin does with your data

Research (formerly News Curator) scans newsletters and the open web to find and stage a weekly roundup's candidates. Read-only against the public web; writes a staged candidate file, not a finished post. Drafting the post is comms (Comms Desk)'s job.

**Reads:**
- **Web** (`WebSearch` + web fetch) — preferred newsletter sites (per your `/setup-news` configuration), the open web for additional coverage.
- **Plugin settings** — `<config-root>/plugins/research.user-context.md` (topic, audience, sources, format).
- **Shared private profile** — `<config-root>/memory/me/identity.md` (read-only). This plugin does not read `voice.md` itself; comms's `post-assembler` agent reads it at draft time.

**Writes:**
- **Staged candidates** — `<config-root>/staged/roundup/[date].md` (summaries, source links, themes) for the user to hand off to comms's `/post`. `/roundup --draft` chains into `/post` directly when comms is installed.
- **Plugin settings** — `<config-root>/plugins/research.user-context.md` (after `/setup-news`).

**Does not:**
- **Post to LinkedIn or any social platform automatically.** Drafts are review-then-paste-by-user.
- **Quote more than 15 words verbatim from any source** — the `news-curator` agent enforces this constraint to respect copyright.
- **Read paywalled content unless you've explicitly subscribed** (per your user-context); paywalled headlines are surfaced but bodies skipped.
- **Send data to any server beyond the WebSearch and web-fetch services Cowork provides.**

## Where data lives

- Immutable plugin references inside the installed plugin directory.
- Optional run archives at `<config-root>/research/runs/[date].md`.
- Shared identity/voice (read-only) under `<config-root>/memory/me/`.

## What gets sent off your machine

- Whatever your authorized WebSearch and web-fetch tools send when invoked. No additional outbound traffic.

## Supported versions

| Version | Supported |
|---------|-----------|
| 0.1.x   | Yes       |

## Reporting a vulnerability

Report privately via GitHub Security Advisories:

https://github.com/BrightWayAI/news-curator/security/advisories/new

Do not open a public issue for security concerns. We aim to respond within 5 business days.
