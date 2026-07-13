---
title: Troubleshooting
tags: [local-ai, troubleshooting, runbook]
created: 2026-06-14
---

# 06 — Troubleshooting

Back to [README](<README.md>) · Prev: [05 - Tailored Plan (24GB M4)](<05 - Tailored Plan (24GB M4).md>) · Next: [07 - Next Steps](<07 - Next Steps.md>)

## `claude-local: command not found`

The shell hasn't loaded the new `~/.zshrc`. Open a new terminal or run `source ~/.zshrc`. Confirm the function exists: `type claude-local`.

## `claude-local` hangs or errors connecting

Check Ollama is up: `curl -s http://localhost:11434/api/version`. If nothing, `brew services restart ollama`. Confirm the model tag in the function matches an installed model (`ollama list`).

## Responses are very slow / first reply takes 10–20s

Normal: the model loads into RAM on first use, then unloads after ~5 min idle. For instant replies during a work session, pre-warm it: `ollama run qwen2.5-coder:14b ""`. Avoid setting `OLLAMA_KEEP_ALIVE=-1` permanently on 24 GB — it pins ~9 GB always.

## Open WebUI won't load on `:3000`

- Is Docker running? `docker info` — if not, `open -a Docker` and wait ~15s.
- Is the container up? `docker ps`. If missing/exited: `docker start open-webui` or `docker logs open-webui`.
- Health check: `curl -s -o /dev/null -w "%{http_code}" localhost:3000` should return `200`.

## Open WebUI shows no models

The container can't reach the host's Ollama. It must use `http://host.docker.internal:11434` (not `localhost`). Verify from inside: `docker exec open-webui curl -s http://host.docker.internal:11434/api/tags`. If empty, recreate the container with the `-e OLLAMA_BASE_URL=...` flag (see [02 - How to Run](<02 - How to Run.md#operate--maintain>)).

## Nothing comes back after a reboot

- **Ollama** should auto-start (`brew services list`). If "stopped": `brew services start ollama`.
- **Open WebUI** only returns if Docker Desktop started at login. Enable it: Docker Desktop → Settings → General → *"Start Docker Desktop when you sign in."* See [01 - Setup Instructions](<01 - Setup Instructions.md#manual-steps-left-to-you>).
- If the Mac slept, enable "never sleep on power."

## Out of memory / system sluggish

A model + Open WebUI + your apps share 24 GB. Check Activity Monitor memory pressure. Use a smaller model (`qwen2.5-coder:7b`) for routine work, or stop the chat container when not needed: `docker stop open-webui`.

## Model quality feels weak on a hard task

Expected — that's the hard 20%. Fall back to cloud `claude`. See [03 - Jobs to be Done](<03 - Jobs to be Done.md>).
