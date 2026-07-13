---
title: Next Steps
tags: [local-ai, roadmap, ideas]
created: 2026-06-14
---

# 07 — Next Steps

Back to [[README]] · Prev: [[06 - Troubleshooting]]

Optional improvements, roughly in order of value.

## Finish the always-on basics

- [ ] Docker Desktop → start at login (so [[02 - How to Run|Open WebUI]] survives reboots)
- [ ] Disable sleep on power
- [ ] (Optional) enable SSH for headless admin
- [ ] Create the Open WebUI admin account

## Try newer / different models

The 14B is a safe daily driver, but worth experimenting:

- [ ] `qwen3-coder` family — newer; check size vs. your 24 GB before pulling
- [ ] A fast `7–8B` for instant everyday Q&A
- [ ] A general (non-coding) model for writing/chat in Open WebUI

Browse `ollama.com/library`; manage with the commands in [[02 - How to Run#Managing models]].

## Quality-of-life

- [ ] Add a `claude-local` variant pinned to a different model (e.g. `claude-local-7b`) for quick tasks
- [ ] Pre-warm script on login (`ollama run <model> ""`) so the first reply is instant
- [ ] Tailscale for secure access to Open WebUI from outside the house **without** exposing ports publicly (the safe alternative to port-forwarding)

## Evaluate (carefully) an always-on agent

The original plan's "OpenClaw" was unverified. If you ever want a 24/7 agent loop pointed at local Ollama:

> [!warning] Vet first
> Confirm the real project, repo, license, and security posture. Never hand it messaging or financial/posting credentials you wouldn't give a stranger. Prefer well-known open-source frameworks that support a custom Anthropic/OpenAI base URL. See [[04 - Original Implementation Plan#Phase 6 optional 24 7 agent|Phase 6]].

## Housekeeping

- [ ] Periodic `brew upgrade ollama` and Open WebUI image refresh
- [ ] Re-pull models occasionally for updated weights
- [ ] Revisit the [[03 - Jobs to be Done|local vs cloud line]] as local models improve
