# DIRMACS

Open-source Rust infrastructure for agentic AI. We build it, we run it, we ship it.

---

## What we do

We write Rust. We build the servers, the execution engines, the coding agents, and the search layers that let AI agents actually do work — not just talk. Long-horizon workflows. Systems that run 24/7. Infrastructure that builds itself, heals itself, and improves itself.

The human stays in the loop. The agent does the heavy lifting. Tools, skills, memory, and the human — all of it works as one system. That's not a pitch; it's how we operate every day.

## The Stack

Our infrastructure is modular by design. Each layer is a standalone Rust crate or service with a clear responsibility. They compose vertically: agent runtime at the base, coding agent on top, config management across everything.

### Core Runtime

- **[ares](https://github.com/dirmacs/ares)** — Agentic AI server. Multi-provider LLM (NVIDIA NIM, Ollama, Groq, Anthropic), structured tool calling, RAG, MCP integration, multi-tenant metering. The runtime layer that manages agents, routes requests, and exposes an OpenAI-compatible API.

- **[eruka](https://eruka.dirmacs.com)** — Context intelligence engine. Structured business knowledge, workspace isolation, 13 MCP tools, the biological memory layer for agents that need to understand what they're working with.

### Developer Tools

- **[pawan](https://github.com/dirmacs/pawan)** — Self-healing CLI coding agent. 29 tools, AST + LSP powers, streaming TUI with command palette, vim keybindings, markdown rendering. Runs on NVIDIA NIM or local MLX. No subscription, no telemetry. **[docs](https://dirmacs.github.io/pawan)**

- **[aegis](https://github.com/dirmacs/aegis)** — Declarative config management. TOML manifests that generate tool configs for the entire stack.

- **[daedra](https://github.com/dirmacs/daedra)** — Self-contained web search MCP server. 7 backends with automatic fallback. Works from any IP. Pure Rust, no Docker, no API keys required. **[docs](https://dirmacs.github.io/daedra)**

- **[nimakai](https://github.com/dirmacs/nimakai)** — NVIDIA NIM model latency benchmarker. Written in Nim. Measures ping, tool-use, and agent task times across all NIM models.

### Platform & Products

- **[thulp](https://github.com/dirmacs/thulp)** — Execution context engineering for AI agents. 11 crates, 311 tests. Unified tool abstraction for local functions, MCP servers, and OpenAPI endpoints. Query DSL, skill workflows, session management. **[docs](https://dirmacs.github.io/thulp)**

- **[dui](https://crates.io/crates/dui-leptos)** — Component library for Leptos WASM frontends. 29 accessible components, published on crates.io.

## DolTARES and Doltdot

**DolTARES** is our Rust server that ties it all together — powered by ares, thulp, and daedra. It handles chat, workflow orchestration, scheduling, channel delivery, self-healing, and long-horizon DAG execution. DolTARES is where the open-source pieces meet production.

**Doltdot** is the AI agent that runs on DolTARES. It's live — handles real tasks, research, development workflows, automated pipelines, communication. We use it internally to run and improve the very infrastructure it sits on.

## DTrain — Our Operating Methodology

We run on **DTrain** — a 6-phase circular lifecycle that takes any operation from manual to autonomous:

**DSprint** (discover) → **DBuild** (develop) → **DLaunch** (deploy) → **DWatch** (monitor) → **DTune** (improve) → **DGrow** (scale)

DIRMACS is its own first client. We're at Sprint 73 of a 90-sprint bootstrap, building toward a self-operating AI platform. Pawan executes the sprints. Ares runs the agents. Eruka holds the context.

## Engineering Principles

- **Rust-first.** Memory safety, performance, correctness. Agentic systems need to be reliable at runtime, not just at demo time.
- **Composability over monoliths.** Each crate does one thing well. They compose through clean interfaces.
- **Verification over speed.** "Autonomous AI execution without verification gates produces confident fiction." Every deployment is proven with actual command output.
- **NVIDIA downstream.** We build on NVIDIA NIM as our primary inference layer. Downstream integrators with upstream compute.
- **Dogfooding.** We run our own agents on our own infra. Pawan improves pawan. Ares serves ares' agents. If it breaks, we feel it first.

## Where we're headed

AGI-approved infrastructure. We're not waiting for AGI to show up — we're building the stack it'll need when it does. Automating startup, software development, research. We're accelerating with a system that runs 24/7 and we're doing it in the open.

---

- [github.com/dirmacs](https://github.com/dirmacs)
- [dirmacs.com](https://www.dirmacs.com)
- [contact@dirmacs.com](mailto:contact@dirmacs.com)
