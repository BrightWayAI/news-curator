---
disable-model-invocation: true
name: roundup
description: Run the weekly news roundup scan — scan, pick candidates, stage them for drafting. Auto-fires on "/roundup", "/ai-roundup", "weekly AI roundup", "do my roundup", "curate this week's post", "weekly LinkedIn post", "run the roundup", "what happened in [topic] this week" (when the user has news-curator configured for that topic). Orchestrates the news-curator subagent; use `--draft` to chain into comms's `/post` for drafting (requires comms installed). Renamed from `ai-roundup` (2026-09-15).
---

<!-- OPENAI-ADAPTER:START -->
## OpenAI host binding

Before acting, read `../../references/openai-portability.md`. That file translates
host-specific tools, agents, artifacts, scheduling, connectors, and config-root
access for ChatGPT and Codex. It overrides concrete Claude/Cowork tool names only;
the workflow, safety gates, and output contract in this skill remain canonical.
<!-- OPENAI-ADAPTER:END -->


See `commands/roundup.md` for the full pipeline workflow.

## When this skill fires automatically

- User runs `/roundup` directly (or the deprecated `/ai-roundup` alias)
- User says: "do my weekly roundup", "curate this week's post", "weekly LinkedIn AI roundup", "run the roundup", "what happened in AI this week" (interpret as roundup intent if topic matches user-context)
- User says "draft my Friday post" / "draft my [day] roundup" if cadence matches user-context
- A scheduled task triggers this skill (when the user wires up scheduling in Cowork)

## Pre-flight check

Before running, confirm `<config-root>/plugins/research.user-context.md` exists and is populated. If missing, route to `/setup-news` first — the pipeline produces generic, off-voice output without context.

## What this skill is *not* for

- Ad-hoc news questions ("what's the latest on X?"). For that, use WebSearch directly.
- Drafting. This skill finds and stages candidates; it does not write the post. Drafting is comms's `/post` command (`--draft` chains into it automatically if comms is installed).
- Cross-topic posts. This pipeline is configured to one topic at a time. To roundup multiple topics, run twice and stitch manually.
