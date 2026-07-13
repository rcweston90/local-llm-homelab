---
title: Setup Instructions (Build Log)
tags: [local-ai, setup, ollama, docker]
created: 2026-06-14
---

# 01 — Setup Instructions (what was actually done)

Back to [[README]] · Next: [[02 - How to Run]]

This is the real build log for the Mac Mini M4 (24 GB), done 2026-06-14. Everything below was run and verified on the machine.

## Starting state (already present)

- Homebrew 5.1.13 ✅
- Node v25.9.0 / npm 11.12.1 ✅
- Claude Code 2.1.177 ✅
- Docker CLI 29.2.0 (daemon not running) ✅
- **Missing:** Ollama

Because Homebrew was already installed, there were **no `sudo`/password blockers**.

## Step 1 — Install Ollama

```bash
brew install ollama
```

Installed **Ollama 0.30.8** — well above the **0.14.0** floor required for the native Anthropic Messages API that lets Claude Code talk to it with no proxy.

## Step 2 — Run Ollama as a boot service

```bash
brew services start ollama
```

Runs now and restarts at login. Verified:

```bash
curl -s http://localhost:11434/api/version    # -> {"version":"0.30.8"}
```

## Step 3 — Pull a model sized to 24 GB

```bash
ollama pull qwen2.5-coder:14b      # ~9 GB
```

14B is the sweet spot for 24 GB (see [[05 - Tailored Plan (24GB M4)]] for the sizing table). Verified with a direct prompt — model replied correctly in ~10s including cold load.

## Step 4 — Wire up `claude-local` (without breaking cloud Claude Code)

> [!important] Design choice
> The original plan put `ANTHROPIC_BASE_URL` globally in `~/.zshrc`, which would hijack **all** Claude Code usage to the local model. Instead, a dedicated `claude-local` function was added so the normal `claude` keeps using the Anthropic cloud.

Appended to `~/.zshrc`:

```bash
# ----- Local AI (Ollama) additions -----
claude-local() {
    ANTHROPIC_BASE_URL=http://localhost:11434 \
    ANTHROPIC_AUTH_TOKEN=ollama \
    ANTHROPIC_MODEL=qwen2.5-coder:14b \
    claude "$@"
}
# ----- End Local AI additions -----
```

> [!note] `OLLAMA_KEEP_ALIVE=-1` was deliberately **not** set. On a 24 GB daily-driver it would pin ~9 GB of RAM permanently, and it wouldn't affect the brew-managed server anyway.

## Step 5 — Verify the full loop

```bash
ANTHROPIC_BASE_URL=http://localhost:11434 ANTHROPIC_AUTH_TOKEN=ollama \
ANTHROPIC_MODEL=qwen2.5-coder:14b claude -p "Reply with exactly: claude-local works"
```

Exited 0 — Claude Code successfully connected to local Ollama. (Output was slightly rough on tool-call formatting; that's the expected local-model tradeoff, see [[03 - Jobs to be Done]].)

## Step 6 — Open WebUI (private chat) via Docker

```bash
open -a Docker      # start the daemon

docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

Verified: container healthy on `:3000`, and from inside the container it sees `qwen2.5-coder:14b` on the host. Reachable on the LAN at `http://<mac-lan-ip>:3000`.

## Manual steps left to you

These are system/security/app settings that must be done by hand:

1. **Docker on login** — Docker Desktop → Settings → General → *"Start Docker Desktop when you sign in."* (Required for the `--restart always` container to come back after reboot.)
2. **Never sleep** — System Settings → Lock Screen / Battery → prevent automatic sleeping on power.
3. **(Optional) SSH** — System Settings → General → Sharing → Remote Login, for headless admin.
4. **Open WebUI admin** — visit `http://localhost:3000` and create the admin account ASAP (first network visitor becomes admin).

See [[02 - How to Run]] for day-to-day usage and [[06 - Troubleshooting]] if something misbehaves.
