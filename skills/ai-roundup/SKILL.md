---
name: ai-roundup
description: Run the full weekly news roundup pipeline — scan, pick candidates, draft a LinkedIn-ready post in your voice. Auto-fires on "/ai-roundup", "weekly AI roundup", "do my roundup", "curate this week's post", "weekly LinkedIn post", "run the roundup", "what happened in [topic] this week" (when the user has news-curator configured for that topic). Orchestrates the news-curator and post-assembler subagents.
---

See `commands/ai-roundup.md` for the full pipeline workflow.

## When this skill fires automatically

- User runs `/ai-roundup` directly
- User says: "do my weekly roundup", "curate this week's post", "weekly LinkedIn AI roundup", "run the roundup", "what happened in AI this week" (interpret as roundup intent if topic matches user-context)
- User says "draft my Friday post" / "draft my [day] roundup" if cadence matches user-context
- A scheduled task triggers this skill (when the user wires up scheduling in Cowork)

## Pre-flight check

Before running, confirm `references/user-context.md` exists and is populated. If missing, route to `/setup-news` first — the pipeline produces generic, off-voice output without context.

## What this skill is *not* for

- Ad-hoc news questions ("what's the latest on X?"). For that, use WebSearch directly.
- Drafting non-roundup posts (single-story posts, opinion pieces). The post-assembler is roundup-shaped; for other formats, use a writing skill.
- Cross-topic posts. This pipeline is configured to one topic at a time. To roundup multiple topics, run twice and stitch manually.
