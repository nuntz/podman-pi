# pi-coding-agent Podman Image

Minimal Podman image that runs the `@mariozechner/pi-coding-agent` CLI via `npx`.

## Prerequisites

- Podman (with a working `podman machine` on macOS/Windows if needed)

## Build

```bash
podman build -t pi-coding-agent .
```

## Run

Mount your repo into `/workspace` and run the agent:

```bash
podman run --rm -it \
  -v "$PWD":/workspace \
  -v "$HOME/.pi/agent/models.json:/root/.pi/agent/models.json:ro" \
  -w /workspace \
  pi-coding-agent
```

## Notes

- The container entrypoint is the `pi` CLI from `@mariozechner/pi-coding-agent`.
- The agent expects its config at `/root/.pi/agent/models.json` and writes sessions to `/root/.pi/agent/sessions` inside the container (ephemeral unless you mount it).
- If your model server runs on the host (e.g. llama.cpp), bind it to `0.0.0.0` and set `baseUrl` to `http://host.containers.internal:PORT/v1` in `~/.pi/agent/models.json`.
- Example llama.cpp host command:

```bash
llama-server \
  -m ~/.llama.cpp/models/Qwen3-4B-Instruct-2507-UD-Q8_K_XL.gguf \
  --host 0.0.0.0 \
  --port 8080 \
  --ctx-size 65536 \
  -ngl 99 \
  --api-key local
```
- npm caches are directed to `/tmp/.npm` to avoid writing into the mounted repo.
