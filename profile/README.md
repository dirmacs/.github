# dirmacs

Rust services for running AI agents. A version ships only after that exact version has been running our own agents in production.

---

## Why

An agent that doesn't know a number will invent one. It also loses what it learned last session and can't tell a verified fact from a guess. Multiplied across tenants and long workflows, the failure isn't a wrong answer — it's that nobody can say where the answer came from.

## What's here

**[eruka](https://eruka.dirmacs.com)** — agent memory with confidence states. Every fact is CONFIRMED, INFERRED, UNCERTAIN, or UNKNOWN. Before generation, eruka writes the gaps into the prompt — *"revenue is UNKNOWN; do not state it."* The model can still ignore a prompt, but the gap is now explicit in the request and logged against the output, which is where fabrication used to hide. **[eruka-mcp](https://github.com/dirmacs/eruka-mcp)** ([crates.io](https://crates.io/crates/eruka-mcp), [docs](https://dirmacs.github.io/eruka-mcp)) connects it to Claude, Cursor, VS Code, and anything else that speaks MCP.

**[ares](https://github.com/dirmacs/ares)** — the runtime. Built on Cordis, a dependency-injection kernel where each capability is its own crate and nothing boots unless its dependencies are wired up. It routes across NVIDIA NIM, Ollama, and Anthropic, meters usage per tenant, and speaks OpenAI so existing clients drop in.

**[thulp](https://github.com/dirmacs/thulp)** — execution context ([docs](https://dirmacs.github.io/thulp)). One interface over local functions, MCP servers, and OpenAPI endpoints, with a query DSL for tool discovery and skill workflows that chain tools into reusable sequences.

**[daedra](https://github.com/dirmacs/daedra)** — web search ([docs](https://dirmacs.github.io/daedra)). Thirteen backends with failover: one goes down or gets rate-limited, the next picks up mid-request.

**[deagle](https://github.com/dirmacs/deagle)** — code intelligence ([docs](https://dirmacs.github.io/deagle)). Tree-sitter plus SQLite in one binary. It indexes a 94-file Rust project in 2.2 seconds and a 14-file one in 125 milliseconds, measured with hyperfine.

**[pawan](https://github.com/dirmacs/pawan)** — the coding agent ([docs](https://dirmacs.github.io/pawan)). AST- and LSP-powered editing, a streaming TUI with vim keys, a tiered model registry that installs its own tools. Runs on NVIDIA NIM in the cloud or MLX on-device. MIT, no telemetry, bring your own model.

**[thulpoff](https://github.com/dirmacs/thulpoff)** — skill distillation. Record a strong model solving a task, extract the reusable pattern into a SKILL.md, then check that a cheaper model can actually follow it.

## Tooling

- **[dstack](https://github.com/dirmacs/dstack)** — multi-repo agent workflows ([crates.io](https://crates.io/crates/dstack), [docs](https://dirmacs.github.io/dstack)): persistent memory, cross-repo sync, quality gates.
- **[dwasm](https://github.com/dirmacs/dwasm)** — Leptos WASM builds ([crates.io](https://crates.io/crates/dwasm), [docs](https://dirmacs.github.io/dwasm)): works around the wasm-opt bulk-memory incompatibility on some rustc/binaryen pairings, handles content hashing.
- **[dui](https://github.com/dirmacs/dui)** — Leptos components ([crates.io](https://crates.io/crates/dui-leptos)): accessible, signal-driven, dark-first.
- **[lancor](https://github.com/dirmacs/lancor)** — llama.cpp toolkit ([docs](https://dirmacs.github.io/lancor)): API client, HuggingFace Hub, server orchestration, benchmarks.
- **[aegis](https://github.com/dirmacs/aegis)** — typed TOML manifests that generate configs for the whole stack.
- **[nimakai](https://github.com/dirmacs/nimakai)** — NIM latency benchmarking, in Nim.

## Open and managed

Everything above is open — clone it, self-host it, it's yours. **[openeruka](https://github.com/dirmacs/openeruka)** is a self-contained Eruka-compatible memory server: SQLite backend, REST and MCP, one binary.

The managed part is the orchestration: ARES, thulp, and daedra wired together for chat, scheduling, long-horizon DAG execution, and channel delivery, with an agent on top. That control plane stays managed, the way Tailscale keeps its coordination server managed — running it safely is the product. The open pieces are the same code we run.

## Working style

- Fixes land with a test that fails without them, or the PR says why not.
- Benchmarks are hyperfine output committed in each repo, with the command line and date, so anyone can rerun and compare.
- Small, single-purpose crates over frameworks — the org has 25 public repos, most under a few thousand lines.
- Our own agents do real work on this infrastructure, so a wrong result is a production incident here before it is anyone else's bug report.

---

[dirmacs.com](https://www.dirmacs.com) · [dirmacs.github.io](https://dirmacs.github.io) · [contact@dirmacs.com](mailto:contact@dirmacs.com)
