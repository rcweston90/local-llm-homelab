---
title: Original Implementation Plan (source)
tags: [local-ai, source, archive]
created: 2026-06-14
source: "@hey_madni thread"
---

# 04 — Original Implementation Plan (source document)

Back to [[README]] · Prev: [[03 - Jobs to be Done]] · Next: [[05 - Tailored Plan (24GB M4)]]

> [!info] This is the original file shared at the start, preserved as-is for reference.
> It was based on a social thread. The corrected, machine-specific version lives in [[05 - Tailored Plan (24GB M4)]], and what was actually built is in [[01 - Setup Instructions]].

---

## The goal

Run an always-on private AI server that handles the bulk of daily AI work locally — Claude Code pointed at a local model, a private ChatGPT-style web UI, and optionally a 24/7 agent — to replace most cloud subscription spend. Keep one cheap frontier subscription for the hard 20%.

**Target end state:** Claude Code against a local model via Ollama, Open WebUI for chat, ~$3/month electricity, hybrid totaling ~$23/month instead of $200–460/month.

## Phase 0 — Decide if it's worth it

The math works only for heavy AI users. Break-even: if you spend $150+/mo on AI and do lots of repetitive non-frontier work, a one-time $599–$1,399 box pays back in a few months. Light users on one $20/mo plan should skip it. A local model won't match complex agentic coding, hard reasoning, or large-context tasks — plan for hybrid, not "local only."

## Phase 1 — Choose and buy hardware

Deciding factors: unified memory size (how big a model fits) and bandwidth (token speed). Apple Silicon shares one memory pool, avoiding the discrete-GPU copy penalty.

| Tier | Approx. price | RAM | Runs | Good for |
|---|---|---|---|---|
| Mac Mini M4 base | ~$599 | 16 GB | up to ~8B | ~70% of daily work |
| Mac Mini M4 | ~$799 | 32 GB | ~14B | real coding; value sweet spot |
| Mac Mini M4 Pro | ~$1,399 | 48 GB | ~70B | closest local-to-frontier |

Recommendation: 32 GB minimum; 48 GB if local Claude Code is the main use. *(These figures were later found to be wrong — see [[05 - Tailored Plan (24GB M4)]].)*

## Phase 2 — First-boot

1. macOS setup, sign in, Software Update.
2. Enable Remote Login (SSH) + optionally Screen Sharing.
3. Set to never sleep on power.
4. Install Homebrew.

## Phase 3 — Install Ollama and pull a model

Ollama is the local runtime; since **v0.14.0** it exposes a native Anthropic Messages API (what makes Claude Code work with no proxy).

```bash
brew install ollama
ollama serve                       # localhost:11434
ollama pull qwen2.5-coder:14b      # size to your RAM tier
```

Optional always-on: `export OLLAMA_KEEP_ALIVE=-1`.

## Phase 4 — Point Claude Code at the local model

```bash
export ANTHROPIC_BASE_URL=http://localhost:11434
export ANTHROPIC_AUTH_TOKEN=ollama   # required but ignored
```

Set the model to your pulled tag; prefer 64k+ context for coding. Expect to fall back to cloud for the hard 20%.

## Phase 5 — Private web chat (Open WebUI)

```bash
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

Open `http://localhost:3000`, create admin, it auto-detects Ollama models. LAN access via `http://<mac-mini-ip>:3000`.

## Phase 6 (optional) — 24/7 agent

The thread describes "OpenClaw," an always-on agent pointed at local Ollama.

> [!warning] Unverified
> "OpenClaw" and the thread's business-income "receipts" could not be independently confirmed. Don't give any always-on agent credentials or financial/posting permissions you wouldn't hand a stranger. Treat income screenshots as anecdotes.

## Phase 7 — Operate it

Run services on boot (`launchd` / Docker `--restart always`); update periodically; monitor health and memory; keep services on localhost/LAN only; the Mini draws ~10–20W.

## The honest cost math (original)

| | Cloud-only | Hybrid |
|---|---|---|
| Subscriptions | ~$200–460/mo | one ~$20/mo plan |
| Electricity | included | ~$2–3/mo |
| Hardware | $0 | $599–$1,399 once |
| **Effective monthly** | **$200–460** | **~$23** |

## What was verified vs. flagged

- **Verified:** Ollama v0.14.0+ exposes the Anthropic Messages API; Claude Code connects via the two env vars; Apple Silicon unified memory avoids the copy penalty.
- **Flagged:** exact Mac prices/bandwidth; specific model names; "OpenClaw" and all income claims (unverified marketing).

*Sources: Ollama Anthropic compatibility docs · Ollama blog (Claude Code) · original thread @hey_madni.*
