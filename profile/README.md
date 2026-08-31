<p align="center"><img src="images/vaelkeep-overview.png" alt="Vaelkeep at a glance — your clients talk to one self-hosted Vaelkeep server, which drives any OpenAI-compatible model, keeps memory in your Obsidian vault, and extends itself through plugins and approved skills — all on hardware you own" width="1000"></p>

# Vaelkeep

**A self-hosted AI agent server with a native-app protocol and a human trust gate for self-extension.** Everything runs on hardware you own — any OpenAI-compatible model, memory as plain markdown in your Obsidian vault, no cloud, no telemetry.

| Component | Repo | What it is |
|---|---|---|
| **Setup** | [`setup`](https://github.com/vaelkeep/setup) | One command that installs the server and the desktop app, and proves it works |
| **Server** | [`server`](https://github.com/vaelkeep/server) | The agent: Pydantic AI core, capability registry, memory pipeline, web search, loops / delegation / cron, skills with an approval gate, AG-UI + REST device protocol |
| **Desktop** | [`desktop`](https://github.com/vaelkeep/desktop) | Electron client for macOS, Windows, and Linux |
| **iOS** | *coming soon* | SwiftUI client for iPhone and iPad |
| **Capabilities** | [`capabilities`](https://github.com/vaelkeep/capabilities) | Optional plugins: weather, YouTube, diagrams, GitHub, code repos, Apple integrations, budgets, travel |

## Install

Three paths to the same server. All you need first is a model endpoint that calls tools reliably — roughly 20B+, via Ollama, llama.cpp, vLLM, LM Studio, or anything speaking the OpenAI API. Smaller models fail at tool calling in ways that look like bugs, so every path checks this for you before you find out the hard way.

### One command

Installs the server, builds and pairs the desktop app, and finishes by asking the agent for a joke. Safe to re-run.

```bash
git clone https://github.com/vaelkeep/setup.git vaelkeep && cd vaelkeep
./install.sh
```

### Docker

No Python toolchain, and the way to run the server on Windows. Self-hosted search is an optional profile.

```bash
git clone https://github.com/vaelkeep/server.git && cd server
cp .env.example .env                            # set LLM_BASE_URL / LLM_MODEL
docker compose up -d --build                    # server only
docker compose --profile search up -d --build   # server + SearXNG
```

### From source

For macOS, where the Apple capability plugins need to run outside a container.

```bash
git clone https://github.com/vaelkeep/server.git && cd server
uv sync && cp .env.example .env
uv run vaelkeep doctor    # endpoint, model, tool calling, persona
uv run vaelkeep serve
```

### Then pair a client

Grab [Vaelkeep Desktop](https://github.com/vaelkeep/desktop/releases) for macOS, Windows, or Linux — or [build it from source](https://github.com/vaelkeep/desktop#-getting-started). Pair it with a code from `vaelkeep pair --host <reachable-host>`, or hand it `VAELKEEP_SERVER_URL` and `VAELKEEP_DEVICE_TOKEN` and it configures itself.

**When something is wrong, ask.** `vaelkeep doctor` checks the endpoint, compares the model you configured against what your endpoint actually serves, confirms the model can call tools, and reports what it finds. It exits non-zero, so scripts and CI can gate on it.

## An app, not a bot

Vaelkeep doesn't live inside Telegram or Slack. Clients pair with your server once and speak an open, documented protocol — [AG-UI](https://docs.ag-ui.com) streaming for turns, REST for pairing, rotating refresh tokens, cross-device sync, push, and approval prompts.

<table align="center"><tr>
<td width="76%" valign="top"><img src="images/desktop-chat.png" alt="Vaelkeep Desktop — a portfolio question answered with a table, streamed from the local server"></td>
<td width="24%" valign="top"><img src="images/ios-chat.png" alt="Vaelkeep iOS app (coming soon) — the New Conversation sheet listing prompt templates served by the server"></td>
</tr></table>
<p align="center"><sub>Vaelkeep Desktop and the Vaelkeep iOS app (coming soon), both paired to one server.</sub></p>

## Self-extension you approve

The agent writes its own skills in the open [agentskills.io](https://agentskills.io) format — but nothing runs unrestricted until it has passed a recorded test, been submitted under a content hash, and been approved by you, out of band, never through the model.

<p align="center"><img src="images/vaelkeep-trust-gate.png" alt="Skill lifecycle: create_skill → test_skill in a restricted harness → submit_skill (refused unless the folder hash matches the tested one) → you approve in the app or CLI → live, with a versioned snapshot and rollback; any edit goes back to pending" width="1000"></p>

## What runs where

<p align="center"><img src="images/vaelkeep-environment.png" alt="Vaelkeep environment — Desktop and other AG-UI clients talking to the Vaelkeep server over AG-UI and the device REST API; the server talking to a model server (Ollama, llama.cpp, vLLM, or any OpenAI-compatible endpoint), a self-hosted SearXNG instance, and the Obsidian vault it shares with the Obsidian app" width="1000"></p>
<p align="center"><sub>Everything inside the dashed boundary runs on one machine. Only Apple push (optional) leaves it.</sub></p>

## Go further

- **Add plugins** — `uv add "vaelkeep-capabilities[all] @ git+https://github.com/vaelkeep/capabilities"`, then `vaelkeep capabilities list`.
- **Build your own client** — [`docs/client-guide.md`](https://github.com/vaelkeep/server/blob/main/docs/client-guide.md) is written so you can hand it to a coding agent and get a working client back.
- **Make it yours** — persona files, private capabilities, and a git-tracked skills folder: [Your Own Vaelkeep](https://github.com/vaelkeep/server#-your-own-vaelkeep-private-layer).

## How is this different?

Self-hosted agents with memory and skills are no longer rare. Vaelkeep's choices: **an app, not a bot** (owned clients over an open protocol instead of messaging gateways); **self-extension you approve**; **memory you can open in Obsidian**; and a **small, typed core** tuned for local models. Full comparison in the [server README](https://github.com/vaelkeep/server#-how-is-this-different).

---

Built by [Rod Moore](https://github.com/rmoore2112) at **CoreAutomation LLC** · MIT licensed
