---
title: How to Run (Operations Runbook)
tags: [local-ai, runbook, ollama, docker]
created: 2026-06-14
---

# 02 — How to Run

Back to [[README]] · Prev: [[01 - Setup Instructions]] · Next: [[03 - Jobs to be Done]]

## What's running

| Service | URL / command | Auto-start |
|---|---|---|
| Ollama (runtime) | `http://localhost:11434` | ✅ `brew services` |
| Model | `qwen2.5-coder:14b` (9 GB) | on demand |
| Claude Code → local | `claude-local` | per session |
| Open WebUI (chat) | `http://localhost:3000` · LAN `http://<mac-lan-ip>:3000` | ✅ Docker* |

\*Only if Docker Desktop starts at login — see [[01 - Setup Instructions#Manual steps left to you]].

## Daily use

**Local Claude Code** (your normal `claude` still uses the cloud):

```bash
cd ~/your-project
claude-local
```

**Private chat UI:** open `http://localhost:3000`. From phone/laptop on the same Wi-Fi: `http://<mac-lan-ip>:3000`.

## Managing models

```bash
ollama list                 # installed models
ollama pull <model>:<tag>   # add one (browse ollama.com/library)
ollama rm <model>:<tag>     # remove one
ollama ps                   # what's loaded in RAM now
```

Sizing for 24 GB: 7–8B = fast · **14B = sweet spot (current)** · 30–32B = tight · 70B = won't fit → use cloud. Full table in [[05 - Tailored Plan (24GB M4)]].

## Operate & maintain

```bash
brew services list            # is ollama running?
brew services restart ollama  # restart it
brew upgrade ollama           # update Ollama

docker ps                     # is open-webui up?
docker restart open-webui     # restart chat UI
docker logs open-webui        # check logs
```

**Update Open WebUI** (data is preserved in the `open-webui` volume):

```bash
docker pull ghcr.io/open-webui/open-webui:main
docker rm -f open-webui
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -v open-webui:/app/backend/data --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

## Health checks

```bash
curl -s http://localhost:11434/api/version           # Ollama alive
curl -s -o /dev/null -w "%{http_code}" localhost:3000 # Open WebUI -> 200
```

Watch **Activity Monitor** memory pressure when running a model + Open WebUI + your usual apps together.

## Security (do not skip)

- Keep ports **11434** and **3000** on the **LAN only**. Never port-forward to the public internet without auth + a reverse proxy.
- Create the Open WebUI admin account immediately — first network visitor becomes admin.

If something breaks → [[06 - Troubleshooting]].
