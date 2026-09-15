---
disable-model-invocation: true
name: ai-roundup
description: Deprecated alias for `/roundup` (renamed 2026-09-15). Auto-fires on "/ai-roundup" only; all other roundup phrasing now routes through the `roundup` skill.
---

# ai-roundup (deprecated alias)

`/ai-roundup` is renamed to `/roundup`. See `skills/roundup/SKILL.md` for the canonical workflow — do not duplicate it here.

## When this skill fires automatically

- User runs `/ai-roundup` directly.

## Behavior

1. Print: "`/ai-roundup` is now `/roundup` — running it for you. Use `/roundup` going forward."
2. Delegate to `commands/roundup.md` and follow it exactly, passing through any flags the user supplied.
