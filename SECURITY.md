# Security Policy

## What this plugin does with your data

News Curator scans newsletters and the open web to produce a weekly roundup post draft. Read-only against the public web; writes drafts inline plus an optional local archive.

**Reads:**
- **Web** (`WebSearch` + web fetch) — preferred newsletter sites (per your `/setup-news` configuration), the open web for additional coverage.
- **Plugin settings** — `<config-root>/plugins/news-curator.user-context.md` (topic, audience, sources, voice/format).
- **Shared private profile** — `<config-root>/memory/me/identity.md` and `voice.md` (read-only).

**Writes:**
- **Drafts** — produced inline in conversation by the `post-assembler` subagent for your review. The post + first-comment + alternate hook.
- **Plugin settings** — `<config-root>/plugins/news-curator.user-context.md` (after `/setup-news`).
- **Optional local archive** — `<config-root>/news-curator/runs/[date].md` if you accept the offer at end of `/ai-roundup`.

**Does not:**
- **Post to LinkedIn or any social platform automatically.** Drafts are review-then-paste-by-user.
- **Quote more than 15 words verbatim from any source** — the `news-curator` agent enforces this constraint to respect copyright.
- **Read paywalled content unless you've explicitly subscribed** (per your user-context); paywalled headlines are surfaced but bodies skipped.
- **Send data to any server beyond the WebSearch and web-fetch services Cowork provides.**

## Where data lives

- Immutable plugin references inside the installed plugin directory.
- Optional run archives at `<config-root>/news-curator/runs/[date].md`.
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
