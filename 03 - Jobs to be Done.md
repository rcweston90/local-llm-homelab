---
title: Jobs to be Done
tags: [local-ai, strategy, jtbd]
created: 2026-06-14
---

# 03 — Jobs to be Done

Back to [[README]] · Prev: [[02 - How to Run]] · Next: [[04 - Original Implementation Plan]]

The point of this server isn't "run AI locally" for its own sake — it's to move the **high-volume, low-difficulty** work off paid cloud services and onto hardware you already own, keeping cloud spend for only the work that truly needs it.

## Primary jobs (do these locally)

> [!success] Local model handles ~80% of daily work for free
> - **Drafting** — emails, notes, first drafts, rewrites
> - **Summarizing** — long docs, threads, transcripts
> - **Everyday coding** — scripts, boilerplate, refactors, explaining code, tests
> - **Q&A** — quick factual/lookup questions, rubber-ducking
> - **Private chat** — anything you'd rather not send to a cloud provider, via Open WebUI

## Keep for the cloud (the hard 20%)

> [!warning] Local models are meaningfully weaker here
> - **Complex multi-step agentic coding** — long tool-use chains, big refactors across many files
> - **Hard reasoning** — tricky logic, math, planning
> - **Large context** — tasks needing very long context windows
> - **Anything where output quality really matters** — client-facing, high-stakes

The deciding habit: start local. If the model stalls, loops, or the quality isn't there, fall back to cloud Claude. Over time you'll learn where your personal 80/20 line sits.

## Why this is worth it on this machine

The hardware is already paid for, so every task that runs locally instead of cloud is straight savings (≈$2–3/mo electricity vs. $200+/mo of stacked subscriptions). The job is **cost reduction + privacy**, not chasing frontier quality. For the economics and the corrected hardware math see [[05 - Tailored Plan (24GB M4)]].

## Known limitation to keep in mind

Ollama's Anthropic API compatibility is new (shipped Jan 2026); streaming and tool-calling edge cases are still being patched. Expect occasional rough edges on tool-heavy `claude-local` sessions — not a reason to avoid it, just a reason to keep cloud handy.
