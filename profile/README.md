<p align="center"><img src="https://raw.githubusercontent.com/enclave-agent/server/main/docs/architecture/enclave-overview.png" alt="Enclave at a glance — your clients talk to one self-hosted Enclave server, which drives any OpenAI-compatible model, keeps memory in your Obsidian vault, and extends itself through plugins and approved skills" width="900"></p>

# Enclave

**A self-hosted AI agent server with a native-app protocol and a human trust gate for self-extension.** Everything runs on hardware you own — any OpenAI-compatible model, memory as plain markdown in your Obsidian vault, no cloud, no telemetry.

| Component | Repo | What it is |
|---|---|---|
| **Server** | [`server`](https://github.com/enclave-agent/server) | The agent: Pydantic AI core, capability registry, memory pipeline, loops / delegation / cron, skills with an approval gate, AG-UI + REST device protocol |
| **Desktop** | [`desktop`](https://github.com/enclave-agent/desktop) | Electron client for macOS, Windows, and Linux |
| **iOS** | `ios` *(coming)* | SwiftUI client for iPhone and iPad |
| **Capabilities** | [`capabilities`](https://github.com/enclave-agent/capabilities) | Optional plugins: weather, YouTube, diagrams, GitHub, code repos, Apple integrations, budgets, travel |

## Start here

1. **Run the server** — [`server` README → Getting Started](https://github.com/enclave-agent/server#-getting-started). Needs Python 3.12+, `uv`, and a model endpoint that calls tools reliably (~20B+ on Ollama, or any OpenAI-compatible server).
2. **Pair a client** — `enclave pair` prints a QR code; scan it from the iOS app or paste it into Desktop.
3. **Add plugins** — `uv add "enclave-capabilities[all] @ git+https://github.com/enclave-agent/capabilities"`.

## Build your own client

The device protocol is open and documented: [AG-UI](https://docs.ag-ui.com) streaming for turns, REST for pairing, rotating refresh tokens, sync, push, and prompts. [`docs/client-guide.md`](https://github.com/enclave-agent/server/blob/main/docs/client-guide.md) is written so you can hand it to a coding agent and get a working client back.

## How is this different?

Self-hosted agents with memory and skills are no longer rare. Enclave's choices: **an app, not a bot** (owned clients over an open protocol instead of messaging gateways); **self-extension you approve** (agent-written skills are test-gated, hash-bound, and human-approved per revision); **memory you can open in Obsidian**; and a **small, typed core** tuned for local models. Full comparison in the [server README](https://github.com/enclave-agent/server#-how-is-this-different).

---

Built by [Rod Moore](https://github.com/rmoore2112) at **CoreAutomation LLC** · MIT licensed
