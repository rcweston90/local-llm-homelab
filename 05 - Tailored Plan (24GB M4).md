---
title: Tailored Plan — 24GB M4
tags: [local-ai, analysis, hardware, cost]
created: 2026-06-14
---

# 05 — Tailored Plan (24 GB M4)

Back to [[README]] · Prev: [[04 - Original Implementation Plan]] · Next: [[06 - Troubleshooting]]

The corrected, machine-specific version of [[04 - Original Implementation Plan]], fixed for the actual hardware: **Mac Mini M4, 24 GB**.

## What the original got wrong

> [!bug] The hardware table had real errors, not just "approximate" numbers
> - **There is no 32 GB Mac Mini M4.** The M4 ships in 16 GB or 24 GB; only the M4 *Pro* goes to 48 GB.
> - **48 GB was badly underpriced** — listed at ~$1,399, but a 48 GB M4 Pro is ~$2,000. The $1,399 is the M4 Pro *24 GB* base.
> - **The $599 base no longer exists** — Apple removed the 256 GB base in May 2026; entry M4 now starts at $799/512 GB.

The 24 GB M4 you have is the **best non-Pro option** and a genuinely good local-inference box.

## What 24 GB actually runs

macOS uses a few GB, so plan on ~16–18 GB usable.

| Model size | Fits? | Experience |
|---|---|---|
| 7–8B (Q4) | Yes, easily | Fast, instant. ~70% of daily work. |
| **14B (Q4)** | **Sweet spot (current)** | ~8–9 GB, comfortable with big context. |
| 30–32B (Q4) | Tight | ~18–20 GB, little headroom; slow for live coding. |
| 70B | No | Needs ~40 GB+. Use cloud — this is the hard 20%. |

## Corrected cost math (your situation)

| | Cloud-only | Your hybrid setup |
|---|---|---|
| Subscriptions | ~$200–460/mo | one ~$20/mo plan |
| Electricity | included | ~$2–3/mo |
| Hardware | $0 | **already paid** (~$999) |
| **Effective monthly** | $200–460 | **~$23** |

Box is bought, so every local month is straight savings. Against $200/mo, the sunk hardware pays back in ~5 months.

## Caveats that apply to you

- Ollama's Anthropic compatibility is new (Jan 2026) — streaming/tool-calling edge cases still being patched.
- Current local coding model families have moved on (qwen3-coder, glm-4.7, minimax-m2.1); `qwen2.5-coder:14b` was chosen as the safe, comfortable dense 14B for 24 GB. Worth re-evaluating when pulling a new model — see [[02 - How to Run#Managing models]].

## Verified facts

Ollama v0.14.0+ exposes the Anthropic Messages API ✅ · Claude Code connects via `ANTHROPIC_BASE_URL` + `ANTHROPIC_AUTH_TOKEN` ✅ · Apple Silicon unified memory avoids the discrete-GPU copy penalty ✅.

*Sources: Ollama Anthropic compatibility docs · Ollama blog (Claude Code) · apple.com Mac mini tech specs.*
