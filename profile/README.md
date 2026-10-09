# dirmacs

Open-source Rust infrastructure for AI agents. Our own agents run on it in production before anyone else uses it.

---

## The problem

AI agents invent facts. They forget what they learned last session. They cannot tell a verified fact from a guess. One wrong answer is bad. Worse is that nobody can say where the answer came from.

We make infrastructure that keeps agents honest.

## The stack

- **[eruka](https://eruka.dirmacs.com)** gives agents memory with confidence states. Each fact is confirmed, inferred, stale, or missing. The agent sees the gaps before it answers.
- **[ares](https://github.com/dirmacs/ares)** runs the agents. It routes across model providers, tracks usage per tenant, and speaks the OpenAI API.
- **[thulp](https://github.com/dirmacs/thulp)** connects agents to tools and multi-step workflows. [docs](https://dirmacs.github.io/thulp)
- **[daedra](https://github.com/dirmacs/daedra)** gives agents web search with automatic fallback across 13 backends. [docs](https://dirmacs.github.io/daedra)
- **[deagle](https://github.com/dirmacs/deagle)** makes a codebase searchable for an agent. [docs](https://dirmacs.github.io/deagle)
- **[pawan](https://github.com/dirmacs/pawan)** is our coding agent. It uses all of the above. [docs](https://dirmacs.github.io/pawan)

## More tools

- **[eruka-mcp](https://github.com/dirmacs/eruka-mcp)**: eruka in Claude, Cursor, and VS Code. [crates.io](https://crates.io/crates/eruka-mcp) · [docs](https://dirmacs.github.io/eruka-mcp)
- **[openeruka](https://github.com/dirmacs/openeruka)**: a self-hosted memory server compatible with eruka. One binary.
- **[thulpoff](https://github.com/dirmacs/thulpoff)**: teaches small models from large ones with reusable skill files.
- **[dstack](https://github.com/dirmacs/dstack)**: persistent memory and quality gates for multi-repo agent work. [crates.io](https://crates.io/crates/dstack) · [docs](https://dirmacs.github.io/dstack)
- **[dwasm](https://github.com/dirmacs/dwasm)**: builds Leptos WASM frontends. [crates.io](https://crates.io/crates/dwasm) · [docs](https://dirmacs.github.io/dwasm)
- **[dui](https://github.com/dirmacs/dui)**: accessible Leptos components. [crates.io](https://crates.io/crates/dui-leptos)
- **[lancor](https://github.com/dirmacs/lancor)**: a llama.cpp toolkit. [docs](https://dirmacs.github.io/lancor)
- **[aegis](https://github.com/dirmacs/aegis)**: typed manifests that generate configuration for the stack.
- **[nimakai](https://github.com/dirmacs/nimakai)**: measures NVIDIA NIM model latency. Written in Nim.

## Open and managed

Everything above is open source. Clone it and run it yourself.

We also run it as a managed service. The orchestration layer stays managed because running it safely is the product. The open parts are the same code we run.

## How we work

- Each fix lands with a test that fails without it.
- Benchmarks live in the repos with the command and the date. Anyone can rerun them.
- Small crates that do one thing.
- Our agents do real work on this stack. A bug here breaks our own work first.

---

[dirmacs.com](https://www.dirmacs.com) · [dirmacs.github.io](https://dirmacs.github.io) · [contact@dirmacs.com](mailto:contact@dirmacs.com)
