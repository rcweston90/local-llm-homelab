---
title: Local AI Server — Home
tags: [local-ai, ollama, mac-mini, moc]
created: 2026-06-14
---

# 🏠 Local AI Server — Home

A private, always-on AI server running on a **Mac Mini M4 (24 GB)**. Runs open-source models locally so most daily AI work happens on-device for free, with a cloud subscription kept only for the hard 20%.

> [!summary] Current status
> Ollama + `qwen2.5-coder:14b`, `claude-local` for Claude Code, and Open WebUI chat are all **installed and running**. A few manual always-on toggles remain — see [01 - Setup Instructions](<01 - Setup Instructions.md#manual-steps-left-to-you>).

## Map of content

- [01 - Setup Instructions](<01 - Setup Instructions.md>) — exactly what was installed and configured (the build log)
- [02 - How to Run](<02 - How to Run.md>) — daily commands, URLs, operations & maintenance
- [03 - Jobs to be Done](<03 - Jobs to be Done.md>) — what this server is *for*; when to use local vs cloud
- [04 - Original Implementation Plan](<04 - Original Implementation Plan.md>) — the source plan this was built from
- [05 - Tailored Plan (24GB M4)](<05 - Tailored Plan (24GB M4).md>) — corrected plan sized to this exact machine
- [06 - Troubleshooting](<06 - Troubleshooting.md>) — fixes for the common failure modes
- [07 - Next Steps](<07 - Next Steps.md>) — optional improvements and ideas
- [09 - Cost Model — Local vs Cloud](<09 - Cost Model — Local vs Cloud.md>) — does local actually cut your cloud AI bill?

## The machine

| | |
|---|---|
| Hardware | Mac Mini M4, 24 GB unified memory |
| macOS | Tahoe 26.2 |
| LAN IP | `<mac-lan-ip>` |
| Set up | 2026-06-14 |

## The one principle

Local handles ~80% of daily work for free. Keep **one cloud subscription** for the hard 20% (complex agentic coding, hard reasoning, large context). See [03 - Jobs to be Done](<03 - Jobs to be Done.md>).
