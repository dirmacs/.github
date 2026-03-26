# DIRMACS

Open-source Rust infrastructure for agentic AI. We build it, we run it, we ship it.

---

## The Problem

AI agents hallucinate. They fabricate data, lose context between sessions, and can't distinguish what they know from what they're guessing. Deploy them at scale — across tenants, across tools, across multi-step workflows — and there's no infrastructure to hold it all together. No memory that persists. No confidence tracking. No constraint on what an agent can and cannot claim.

We're building that infrastructure. In Rust. In the open.

## How It All Fits Together

### Memory: Eruka

It starts with **[Eruka](https://eruka.dirmacs.com)** — a context intelligence engine that gives AI agents structured, stateful memory.

Every piece of business context gets a confidence state: **CONFIRMED** (user verified, ground truth), **INFERRED** (AI extracted, high confidence), **UNCERTAIN** (was confirmed, now stale), or **UNKNOWN** (needed but missing). This isn't metadata — it's enforced. Before an agent generates content, Eruka checks readiness and injects constraints into the system prompt: *"DO NOT fabricate: revenue figures. This field is UNKNOWN."* The agent literally cannot hallucinate data it doesn't have.

Eruka provides workspace isolation for multi-tenant deployments, a knowledge graph with typed relationships and temporal validity, gap detection that identifies what's missing before generation begins, a quality scoring pipeline that catches contradictions and ungrounded claims, and a three-tier memory system (core, working, archival) with automatic staleness detection and reclassification.

The bridge between Eruka and the AI tools people actually use is **[eruka-mcp](https://github.com/dirmacs/eruka-mcp)** — an MCP (Model Context Protocol) server that connects Claude, Cursor, VS Code, and any MCP-compatible client to Eruka's knowledge states. Install from [crates.io](https://crates.io/crates/eruka-mcp), point at your Eruka instance, and your AI assistant gains structured memory with anti-hallucination guarantees. Tier-gated tools, service key authentication, input validation, and scope enforcement are built in. **[docs →](https://dirmacs.github.io/eruka-mcp)**

### Runtime: ARES

The agents themselves run on **[ARES](https://github.com/dirmacs/ares)** — a battle-ready agentic AI server. ARES routes requests across inference providers (NVIDIA NIM, Ollama, Anthropic), manages structured tool calling with retry logic, handles RAG with document ingestion, integrates MCP servers as first-class tool providers, and meters usage per tenant with quota enforcement. It exposes an OpenAI-compatible API, so any client that speaks OpenAI can use it without modification. Multi-tenant by default — each tenant gets isolated agents, keys, and usage tracking.

### Context Engineering: Thulp

Agents need more than an LLM and a database. They need to discover tools, validate inputs, follow multi-step workflows, and maintain session context across turns. **[Thulp](https://github.com/dirmacs/thulp)** handles execution context engineering — a unified abstraction over local Rust functions, MCP servers, and OpenAPI endpoints. It provides a query DSL for tool discovery, skill workflows that chain tools into reusable sequences, and session management that tracks state across agent turns. Thulp is the layer that makes agents *composable* — skills built from tools, workflows built from skills. **[docs →](https://dirmacs.github.io/thulp)**

### Search: Daedra

Every agent eventually needs to search the web. **[Daedra](https://github.com/dirmacs/daedra)** is a self-contained web search MCP server with multiple backends and automatic fallback. Pure Rust. No Docker. No API keys required. Works from any IP, any network. When one backend is down or rate-limited, Daedra transparently fails over to the next. Plug it into any MCP-compatible agent and it gains web search without configuration. **[docs →](https://dirmacs.github.io/daedra)**

### The Coding Agent: Pawan

When you need an AI agent that writes and fixes code using all of this infrastructure, there's **[Pawan](https://github.com/dirmacs/pawan)** — a self-healing CLI coding agent. AST and LSP-powered tooling for precise code understanding. Streaming TUI with command palette, vim keybindings, and inline markdown rendering. Tiered model registry with automatic tool installation. Runs on NVIDIA NIM for cloud inference or local MLX for on-device. No subscription, no telemetry, no lock-in. Named after Power Star Pawan Kalyan. **[docs →](https://dirmacs.github.io/pawan)**

## The Supporting Stack

The core wouldn't hold together without the tooling around it:

- **[dwasm](https://github.com/dirmacs/dwasm)** — Production WASM build tool for Leptos frontends. Replaces `trunk build --release` with a five-stage pipeline that handles the wasm-opt bulk-memory compatibility issue that breaks modern Rust WASM builds, automates content hashing for cache busting, and patches index.html references. On [crates.io](https://crates.io/crates/dwasm). **[docs →](https://dirmacs.github.io/dwasm)**

- **[DUI](https://github.com/dirmacs/dui)** — Component library for Leptos WASM frontends. Accessible, signal-driven components with ARIA roles, keyboard navigation, and focus management. Dark-first design system with CSS custom properties. On [crates.io](https://crates.io/crates/dui-leptos). Powers every DIRMACS frontend — the admin dashboard, the Eruka dashboard, the client portals.

- **[Lancor](https://github.com/dirmacs/lancor)** — End-to-end llama.cpp toolkit in Rust. API client for llama.cpp servers, HuggingFace Hub integration for model discovery and download, server orchestration for managing llama.cpp instances, and a benchmark suite for measuring inference performance. **[docs →](https://dirmacs.github.io/lancor)**

- **[Aegis](https://github.com/dirmacs/aegis)** — System configuration manager. Typed TOML manifests that generate tool configs for the entire DIRMACS stack — dotfiles, infrastructure settings, model registries, agent configurations.

- **[Nimakai](https://github.com/dirmacs/nimakai)** — NVIDIA NIM model latency benchmarker. Written in Nim. Measures ping latency, tool-use response time, and full agent task completion time across all available NIM models. Used internally to select the right model for each agent workload.

## How We Operate

### DolTARES and Doltdot

**DolTARES** is our Rust orchestration server — where the open-source pieces meet production. Powered by ARES, Thulp, and Daedra, it handles chat, workflow orchestration, scheduling, channel delivery (including WhatsApp via our Go bridge), self-healing, and long-horizon DAG execution. Declarative TOML-based DAGs define workflows as node graphs with aggregation, conditional branching, and runtime parameters.

**Doltdot** is the AI agent that runs on DolTARES. It's live in production — handling real tasks, research, development workflows, automated pipelines, and communication. We use it internally to run and improve the very infrastructure it sits on. The agent that builds itself.

### DTrain — Our Operating Methodology

We run on **DTrain** — a 6-phase circular lifecycle that takes any operation from manual to autonomous:

**DSprint** (discover) → **DBuild** (develop) → **DLaunch** (deploy) → **DWatch** (monitor) → **DTune** (improve) → **DGrow** (scale) → repeat

DIRMACS is its own first client. Pawan executes the sprints. ARES runs the agents. Eruka holds the context. DolTARES orchestrates the workflows. Every piece of infrastructure serves every other piece.

## Engineering Principles

- **Rust-first.** Memory safety, performance, correctness. Agentic systems need to be reliable at runtime, not just at demo time. We run on a single VPS — every byte matters, every panic is felt.
- **Composability over monoliths.** Each crate does one thing well. They compose through clean interfaces — Eruka doesn't know about ARES, ARES doesn't know about Thulp, but they all work together through MCP and structured APIs.
- **Verification over speed.** "Autonomous AI execution without verification gates produces confident fiction." Every deployment is proven with actual command output, not assumed from passing CI.
- **NVIDIA downstream.** We build on NVIDIA NIM as our primary inference layer. Downstream integrators with upstream compute.
- **Dogfooding.** We run our own agents on our own infra. Pawan improves pawan. ARES serves ARES's agents. Doltdot builds the infrastructure Doltdot runs on. If it breaks, we feel it first.

## Where We're Headed

AGI-approved infrastructure. We're not waiting for AGI to show up — we're building the stack it'll need when it does. Structured memory that scales. Agents that can't lie about what they don't know. Workflows that run unsupervised for days. We're doing it in Rust, we're doing it in the open, and we're doing it on a single VPS that runs 24/7.

---

[github.com/dirmacs](https://github.com/dirmacs) · [dirmacs.com](https://www.dirmacs.com) · [contact@dirmacs.com](mailto:contact@dirmacs.com)
