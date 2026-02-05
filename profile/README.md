# DIRMACS

Open-source Rust infrastructure for agentic AI. We build it, we run it, we ship it.

---

## What we do

We write Rust. We build the servers, the execution engines, and the search layers that let AI agents actually do work — not just talk. Long-horizon DAG workflows. Systems that run 24/7. Infrastructure that builds itself.

The human stays in the loop. The agent does the heavy lifting. Tools, skills, memory, and the human — all of it works as one system. That's not a pitch; it's how we operate every day.

## Architecture

Our stack is modular by design. Each layer is a standalone Rust crate or service with a clear responsibility and a well-defined interface. They compose vertically: agent runtime at the base, execution context in the middle, external capability (search, MCP) at the edge.

The result is a system where you can swap providers, add tools, change models, or extend workflows without rewriting the core. That composability is intentional — it's what lets us iterate fast and ship with confidence.

## Open-source projects

- **[ares](https://github.com/dirmacs/ares)** — Agentic chatbot server. Multi-provider LLM support (cloud and local), structured tool calling, retrieval-augmented generation, and native MCP integration. Ares is the runtime layer: it manages agents, routes requests, handles provider failover, and exposes an OpenAI-compatible API.

- **[thulp](https://github.com/dirmacs/thulp)** — Execution Context Engineering Platform for AI agents. Thulp gives agents structured context: skills, memory protocols, environment awareness, and identity. It's the layer between "the model can generate text" and "the agent knows what it's doing and why."

- **[daedra](https://github.com/dirmacs/daedra)** — Web search and research MCP server. High-performance, Rust-native, DuckDuckGo-powered. Daedra gives agents the ability to search the web and fetch page content in real time — available as a tool or as a standalone MCP server for any compatible client.

## DolTARES and Doltdot

**DolTARES** is our Rust server that ties it all together — powered by ares, thulp, and daedra. It handles chat, workflow orchestration, scheduling, channel delivery, self-healing, and long-horizon DAG execution. DolTARES is where the open-source pieces meet production.

**Doltdot** is the AI agent that runs on DolTARES. It's live. It handles real tasks — research, development workflows, automated pipelines, communication. We use it internally to run and improve the very infrastructure it sits on. That feedback loop is the core of how we work: build the stack, run the agent, learn from production, ship the improvement.

## Engineering principles

- **Rust-first.** Memory safety, performance, and correctness are non-negotiable. We don't reach for Rust because it's fashionable; we reach for it because agentic systems need to be reliable at runtime, not just at demo time.
- **Composability over monoliths.** Each crate does one thing well. Ares doesn't know about Daedra. Thulp doesn't know about Ares. They compose through clean interfaces.
- **Long-horizon by default.** Our DAG engine is built for workflows that run for minutes, hours, or days — not just request-response. Checkpointing, recovery, and self-healing are first-class concerns.
- **Human in the loop.** The agent proposes, the human reviews. Automation without oversight is a liability. We build for the case where someone is watching and the case where they step away.
- **Dogfooding.** We run our own agent on our own infra. If it breaks, we feel it first. That's the fastest way to build something real.

## Where we're headed

AGI-approved infrastructure. We're not waiting for AGI to show up — we're building the stack it'll need when it does. Automating startup, software development, research. We're accelerating with a mini AGI system that runs 24/7 and we're doing it in the open. The work is rigorous, the iteration is fast, and the ambition is deliberate.

---

- [github.com/dirmacs](https://github.com/dirmacs)
- [dirmacs.com](https://www.dirmacs.com)
- [contact@dirmacs.com](mailto:contact@dirmacs.com)
