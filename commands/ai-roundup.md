---
description: Deprecated. Renamed to `/roundup` (2026-09-15) — same behavior, kept as a separate command so existing muscle memory, schedules, and router intent mappings keep working.
---

# /ai-roundup

`/ai-roundup` has been renamed to `/roundup`. This is a thin alias: rewrite the invocation as `/roundup` (passing through any flags, e.g. `--auto-pick`) and follow that command's workflow exactly — same steps, same subagent delegation, same output contract. Do not duplicate the pipeline here; `commands/roundup.md` is the single source of truth.

Print a one-line deprecation notice before proceeding:
> "`/ai-roundup` is now `/roundup` — running it for you. Use `/roundup` going forward."

```
/ai-roundup
```
is equivalent to
```
/roundup
```
