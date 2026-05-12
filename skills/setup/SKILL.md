---
name: setup
description: Configure news-curator for your topic, audience, sources, and voice. Auto-fires on "set up news-curator", "configure roundup", "/setup-news", or any phrase about getting the news roundup pipeline ready. Also fires when the roundup pipeline reports user-context.md is missing.
---

See `commands/setup-news.md` for the full interview.

## When this skill fires automatically

- User runs `/setup-news` directly
- User says: "set up news-curator", "configure my roundup", "set up the AI roundup", "set up my weekly roundup"
- User installs the plugin and asks "how do I use this?"
- The `/ai-roundup` pipeline reports `user-context.md` missing → auto-route here

## Quick path

If the user wants minimum-viable defaults to start: write a placeholder `<config-root>/plugins/news-curator.user-context.md` with topic="AI" and audience="leaders and operators," voice descriptors blank, sources empty (open-web scan only). Note in your reply that the agents will work but produce generic output until the user fills in voice/sources, and recommend running the full interview when they have 5 minutes.

Don't auto-write defaults silently — only on explicit request.
