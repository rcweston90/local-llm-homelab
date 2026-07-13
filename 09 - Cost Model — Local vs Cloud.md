---
title: Cost Model — Local vs Cloud
tags: [local-ai, cost, strategy]
created: 2026-06-14
---

# 09 — Cost Model: does local cut Anthropic spend?

Back to [README](<README.md>) · Related: [03 - Jobs to be Done](<03 - Jobs to be Done.md>)

## The one rule

> [!important] Local only saves money on work you actually move *off* Anthropic.
> Local inference is ~free (≈$2–3/mo electricity). But it doesn't reduce a single Anthropic invoice unless that task would otherwise have hit the Anthropic API/subscription. The savings = (volume you shift to local) × (what that volume cost on Anthropic).

So the question isn't "is local cheaper" (it is) — it's **"how much of your Anthropic usage can the local 14B actually handle?"**

## Where the Anthropic bill actually comes from

Three very different cost profiles:

| Usage type | Cost behavior | Local helps? |
|---|---|---|
| **Flat subscription** (Claude.ai Pro/Max) | Fixed monthly, no metering | Only if local lets you **drop/downgrade** the plan |
| **Interactive Claude Code** (subscription or API) | Moderate, bursty | Partially — shift routine sessions to `claude-local` |
| **Always-on / agentic API** (always-on 24/7) | **Metered, can balloon** | **Biggest win** — this is where local pays off |

The expensive, scary line item is almost always the **metered, high-volume** one: an agent looping in the background, large contexts, many tool calls. That's exactly what a flat subscription doesn't cover and what runs up an API bill.

## Where local wins most

1. **Autonomous agents → huge.** An always-on agent on the Anthropic API is the classic runaway-bill scenario (it works while you sleep = it bills while you sleep). Pointed at local Ollama, that 24/7 background work costs ~$0. **This is your single biggest lever.**
2. **High-volume routine Claude Code → moderate.** Drafting, boilerplate, summarizing, simple refactors via `claude-local` instead of cloud.
3. **Private chat (Open WebUI) → replaces a ChatGPT/Claude seat** for everyday Q&A.

## Where local does NOT help

- **Hard 20%** — complex multi-step agentic coding, hard reasoning, big-context tasks. The 14B is meaningfully weaker; forcing it here costs you *time and rework*, not money saved. Keep cloud for these.
- **If your flat subscription stays.** Moving 30% of chats to local saves $0 if you still pay for Claude Max anyway. **Savings are only realized when you actually cancel or downgrade** a paid plan, or avoid metered API calls.

## Honest expectation for THIS setup (24 GB M4, qwen2.5-coder:14b)

- Realistically offloads **~60–80% of routine volume** (everyday coding, drafting, summarizing, agent background tasks).
- Keep **one** cloud plan (~$20/mo) for the hard 20%.
- **Net effect on the Anthropic bill depends entirely on your action:** the hardware + local model give you the *option* to cut spend; the saving happens when you (a) route always-on agents to local, and (b) cancel or downgrade the cloud plans the local model now covers.

## A simple way to measure it

1. Note your current monthly Anthropic spend (subscriptions + any API).
2. For 2 weeks, default to `claude-local` / the local model; only escalate to cloud when local visibly falls short.
3. Track how often you escalate. If you rarely do, **downgrade/cancel** the now-redundant plan — that cancellation is the actual saving.
4. Re-evaluate as local models improve (see [07 - Next Steps](<07 - Next Steps.md>)).

## Bottom line

Local is a **cost-avoidance tool, not an automatic discount.** It will drive down Anthropic costs **if** you (1) move your metered/always-on agent work to the local model — the biggest win — and (2) actually shed the cloud plans the local model replaces. The hardware's already paid for, so every task you keep local from here is margin.
