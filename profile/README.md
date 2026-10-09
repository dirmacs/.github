# dirmacs

Rust infrastructure for AI agents that have to be right. We run it in production, on our own box, before anyone else touches it.

---

## The problem

An agent that doesn't know a number will invent one. It loses what it learned last session, can't tell a verified fact from a guess it made three turns ago, and has no way to say "I don't have that." Spread that across tenants and multi-step workflows and the failure isn't a wrong answer — it's that nobody can say where the answer came from.

We build the layer underneath that catches this. In Rust. In the open.

## The stack

**[eruka](https://eruka.dirmacs.com)** — memory that refuses to guess. Every fact an agent might use carries a state: CONFIRMED, INFERRED, UNCERTAIN, or UNKNOWN. Before generation, eruka checks what's known and writes the gaps into the prompt — *"revenue is UNKNOWN; do not state it."* The model can still ignore a prompt, but now the gap is explicit in the request and logged against the output, which is where fabrication used to hide. Typed knowledge graph, temporal validity, workspace isolation for tenants, gap detection that flags what's missing before anyone asks. **[eruka-mcp](https://github.com/dirmacs/eruka-mcp)** ([crates.io](https://crates.io/crates/eruka-mcp), [docs](https://dirmacs.github.io/eruka-mcp)) connects it to Claude, Cursor, VS Code, and anything else that speaks MCP.

**[ares](https://github.com/dirmacs/ares)** — the runtime. Built on Cordis, a dependency-injection kernel where each capability is its own crate and nothing boots unless its dependencies wired up. Routes across NVIDIA NIM, Ollama, and Anthropic; meters usage and enforces quotas per tenant; speaks OpenAI, so existing clients drop in. Multi-tenant from the first request, not bolted on after.

**[thulp](https://github.com/dirmacs/thulp)** — execution context ([docs](https://dirmacs.github.io/thulp)). Tool discovery over local functions, MCP servers, and OpenAPI endpoints through one interface; a query DSL for finding the right tool; skill workflows that chain tools into reusable sequences; session state across turns. This is what makes agents composable — skills from tools, workflows from skills.

**[daedra](https://github.com/dirmacs/daedra)** — web search ([docs](https://dirmacs.github.io/daedra)). Thirteen backends with automatic failover. One goes down or gets rate-limited, the next picks up mid-request. Single binary, no API key needed for basic search.

**[deagle](https://github.com/dirmacs/deagle)** — code intelligence ([docs](https://dirmacs.github.io/deagle)). Tree-sitter plus SQLite, one binary. Symbol search, relationship tracing, architecture queries. Indexes a 94-file Rust project in 2.2 seconds, a 14-file one in 125 milliseconds — hyperfine, not vibes.

**[pawan](https://github.com/dirmacs/pawan)** — the coding agent ([docs](https://dirmacs.github.io/pawan)). AST- and LSP-powered editing, a streaming TUI with vim keys, a tiered model registry that installs its own tools. Runs on NVIDIA NIM in the cloud or MLX on-device. MIT, no telemetry, bring your own model.

**[thulpoff](https://github.com/dirmacs/thulpoff)** — skill distillation. Record a strong model solving a task, extract the reusable pattern into a SKILL.md, then check that a cheaper model can actually follow it. If the small model matches on that task, you keep the skill and stop paying for the big one.

## Tooling we lean on

- **[dstack](https://github.com/dirmacs/dstack)** — multi-repo agent workflows ([crates.io](https://crates.io/crates/dstack), [docs](https://dirmacs.github.io/dstack)). Persistent memory, cross-repo sync, quality gates, deploys with rollback.
- **[dwasm](https://github.com/dirmacs/dwasm)** — Leptos WASM builds ([crates.io](https://crates.io/crates/dwasm), [docs](https://dirmacs.github.io/dwasm)). Works around the wasm-opt bulk-memory incompatibility that breaks some rustc/binaryen pairings, handles content hashing and index patching.
- **[dui](https://github.com/dirmacs/dui)** — Leptos components ([crates.io](https://crates.io/crates/dui-leptos)). Accessible, signal-driven, dark-first. Runs every dirmacs frontend.
- **[lancor](https://github.com/dirmacs/lancor)** — llama.cpp toolkit ([docs](https://dirmacs.github.io/lancor)). API client, HuggingFace Hub, server orchestration, benchmarks.
- **[aegis](https://github.com/dirmacs/aegis)** — system configuration. Typed TOML manifests that generate configs for the whole stack.
- **[nimakai](https://github.com/dirmacs/nimakai)** — NIM latency benchmarking, in Nim. We use it to pick the model for each workload.

## Open and managed

The OSS repos above are the floor — clone them, self-host them, they're yours. **[openeruka](https://github.com/dirmacs/openeruka)** is a self-contained Eruka-compatible memory server you can run yourself: SQLite backend, REST and MCP, one binary.

What we run as a service is the part that's expensive to run well. Our managed orchestration layer wires ARES, thulp, and daedra together for chat, scheduling, long-horizon DAG execution, channel delivery, and self-healing, with an agent living on top of it. That control plane stays managed, the way Tailscale keeps its coordination server managed: running it safely is the product. If you'd rather operate it yourself, the open pieces are the real thing, not a demo tier.

## How we work

- **Rust, and one box.** Memory safety and correctness matter because a wrong agent output is worse than a slow one. Everything runs on a single VPS we operate ourselves — every byte matters, and we feel every panic.
- **Each crate does one thing.** They compose over MCP and structured APIs rather than reaching into each other — no circular dependencies. When we found two ways to do the same thing in one of them, we treated it as debt, not choice.
- **Verification, then speed.** A deployment is proven with real command output, not assumed from green CI. We benchmark with hyperfine and show the numbers in each repo, because "it feels fast" is how you ship a regression.
- **We eat it first.** Our own agent builds the infrastructure it runs on. pawan improves pawan. If it breaks, it breaks on us.

## Where this goes

Agents that say what they know and stop at what they don't. Memory that holds its shape across sessions instead of decaying into confident fiction. Workflows that run unsupervised for days and can show their work when they finish.

---

[dirmacs.com](https://www.dirmacs.com) · [dirmacs.github.io](https://dirmacs.github.io) · [contact@dirmacs.com](mailto:contact@dirmacs.com)
