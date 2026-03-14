# Coder — Software Engineer, AI Tools

## Role Summary
- **Link**: https://coder.com/careers (HN Who's Hiring, February 2026)
- **Location**: Remote (US/Canada/UK/Ireland/Poland)
- **Comp**: Not listed (competitive — 100k+ GitHub star project, well-funded)
- **Fit Score**: 5/5

## Lead Profile
Lead with **Developer Tools / Infrastructure** hybrid. Coder's product is cloud development environments, and the AI Tools role is about making those CDEs work for autonomous coding agents — sandboxed execution, monitoring, integration surfaces. The candidate's bilrost (sandboxed execution for agents), Vindler (agent runtime), and qortex-observe (monitoring) map directly. The open-source angle reinforces the DevTools profile: Coder has 100k+ GitHub stars, and the candidate publishes on PyPI, runs CI conformance testing across 7 frameworks, and builds documentation sites. This is someone who thinks about developer experience as a first-class concern.

## Tailored Pitch (2-3 sentences)
I build the infrastructure that makes autonomous agents safe to run in development environments: a hardened VM sandbox (bilrost) with per-tool network isolation and gated filesystem sync, a production agent runtime (Vindler), and OTel instrumentation tracing every agent action. The MCP server and 7 framework adapters mean I've already solved the integration problem Coder's AI Tools team faces — how do you build a platform that works across diverse agent runtimes without forcing each one to learn a new API? I ship open-source tooling with CI conformance testing and PyPI distribution, which matches the rigor a 100k-star project demands.

## Resume Emphasis
1. **bilrost** — Hardened VM sandbox for agent containment. Lima VM, OverlayFS, per-tool network isolation, gated sync. Single CLI command provisioning. Published on PyPI. This is Coder's CDE problem in miniature: safely isolating autonomous code execution with controlled access to external resources.
2. **Framework adapters** — 7 frameworks (CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, LangChain.js), all passing framework-authored test suites. 100+ CI tests with zero-skip policy and latest-version pinning. Demonstrates the platform integration discipline Coder needs for supporting diverse AI agents.
3. **MCP server** — Cross-language stdio transport, Python-TypeScript bridge, 29 tool calls in 3.94s. MCP is the protocol autonomous coding agents use to connect to external services. Direct experience building the transport layer.
4. **qortex-observe** — Full OTel pipeline (Grafana, Jaeger, Prometheus). Traces from invocation through execution through feedback. Monitoring autonomous agents is a core Coder requirement — you can't host what you can't observe.
5. **Vindler (openclaw)** — Production agent runtime, 90 merged PRs. The agent lifecycle layer: start, run, dispatch tools, terminate. Maps to Coder's agent hosting infrastructure.
6. **Open-source practice** — PyPI publishing (qortex, bilrost), mkdocs documentation sites, CI that installs latest framework versions and breaks immediately on upstream changes. Evidence of open-source engineering standards matching a 100k-star project.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| Cloud Development Environments / sandboxed execution | bilrost: Lima VM + OverlayFS + Docker dual-container isolation + gated sync. Per-tool network policies. STRIDE threat model. Single-command provisioning. Published on PyPI. | strong |
| AI agent infrastructure | Vindler (agent runtime), MCP server (agent communication), framework adapters (agent integration). Three layers of agent infrastructure covering runtime, transport, and ecosystem compatibility. | strong |
| Monitoring autonomous agents | qortex-observe: OTel spans on every agent action. Grafana dashboards for latency and feedback rates. Jaeger for per-invocation traces. Posterior drift monitoring. | strong |
| TypeScript | Strong TypeScript. LangChain.js adapter, Mastra adapter, MCP stdio transport — all TypeScript. Daily cross-language development. | strong |
| Python | Primary language across all projects. qortex, bilrost, Vindler, framework adapters all Python. Published packages on PyPI. | strong |
| Go | No production Go experience. Actively learning Rust (planning Rust runtime). Go is the natural next systems language — simpler syntax, goroutine model maps to async agent workloads. | gap |
| Open-source engineering | PyPI packages, mkdocs documentation sites, CI with latest-version pinning and zero-skip policy. 7 adapters passing upstream test suites. 100+ CI tests. | strong |
| Integration surfaces / protocol design | MCP server: 29 tool calls in 3.94s over stdio. Framework adapters: implement native interface so users swap one import. Design principle: don't make users learn a new API. | strong |

## Why This Role (authentic)
Coder is defining how AI agents operate inside development environments, and that's the exact problem I've been solving at smaller scale. bilrost exists because I asked "how do you let an autonomous agent execute code without it escaping the sandbox, exfiltrating data, or corrupting the filesystem?" Coder is asking the same question for thousands of developers and their coding agents simultaneously. The open-source angle matters to me — I publish on PyPI, I write documentation sites, I run CI that breaks when upstream frameworks ship changes. Working on a project with 100k+ GitHub stars means that engineering discipline has real consequences: a broken adapter doesn't just fail my tests, it fails someone's production workflow. That accountability is what I want.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No Go experience (Coder's primary backend language) | Strong Python and TypeScript, actively learning Rust. Go is syntactically simpler than Rust and the concurrency model (goroutines/channels) maps to the async agent patterns I've already built. The ramp to production Go is weeks, not months. |
| No experience on a large open-source project with external contributors | Published two PyPI packages, run CI conformance testing, write documentation sites. The engineering practices are there — the gap is operating at 100k-star scale with external PRs, issue triage, and release management. That's the experience I'd gain. |
| No production multi-tenant CDE experience | bilrost is single-tenant by design (one VM per agent). The containment patterns transfer to multi-tenant isolation, but the operational complexity of hosting thousands of concurrent CDEs is new territory. Frame as: I bring the security model, Coder brings the scale. |
| No enterprise cloud infrastructure (AWS/GCP/Azure) in the portfolio | All infrastructure is local-first (Lima VM, Docker). The abstractions transfer, but I haven't operated cloud infrastructure at enterprise scale. The trajectory from bilrost's local VM to cloud-hosted CDEs is architecturally natural. |

## If They Ask X, Say Y
- **"Walk us through how you'd design a sandboxed environment for an autonomous coding agent."** -- Start with the threat model. I used STRIDE for bilrost: what can the agent exfiltrate (network), what can it corrupt (filesystem), what can it exhaust (resources), what can it escalate (permissions)? Then build walls for each. Lima VM provides the outer process boundary. OverlayFS prevents persistent filesystem writes unless explicitly synced. UFW rules give each tool its own network policy — the agent can hit the LLM API but not arbitrary endpoints. Gated sync means code changes don't leave the sandbox until a human or a policy approves them. For Coder's CDEs, the same layered approach applies at a different scale: the question is which isolation primitive (container, microVM, namespace) gives the right security/performance tradeoff for concurrent coding agents.

- **"How do you think about supporting multiple AI agent frameworks?"** -- Don't make them learn your API. Implement their interface. qortex ships adapters for 7 agent frameworks, and the design principle is: one import swap. If you're using CrewAI, you import qortex's CrewAI tool instead of CrewAI's default retrieval, and everything else works — same method signatures, same return types, same error handling. We pass CrewAI's own test suite, not ours. For Coder, the same principle applies: a CDE for autonomous agents should expose the interfaces agents already expect (filesystem, terminal, MCP), not force each agent framework to build a Coder-specific integration.

- **"Tell us about your open-source engineering practices."** -- CI runs against latest upstream framework versions with zero-skip policy. If CrewAI ships a breaking change on Monday, our CI fails on Monday — not when a user reports it three weeks later. Both qortex and bilrost are published on PyPI with proper packaging. Documentation sites on GitHub Pages via mkdocs. The principle: treat your own tools with the same rigor you'd expect from tools you depend on.

- **"You don't know Go. How would you ramp up?"** -- I switch between Python and TypeScript daily and I'm actively learning Rust. Go is syntactically simpler than Rust with a more constrained type system. The concurrency model — goroutines and channels — maps directly to the async agent workload patterns I've already built. I'd expect to be writing production Go within the first few weeks. The harder ramp is Coder's codebase and architecture decisions, not the language.

- **"What does monitoring autonomous agents look like?"** -- Every agent action becomes an OTel span with structured attributes: what tool was called, what arguments were passed, what the result was, how long it took, whether the agent considered it successful. Grafana shows aggregates — P50/P99 latency, tool call success rates, agent session duration. Jaeger shows individual traces — you can follow one agent session from start to finish and see every decision it made. For Coder, the monitoring question is: when an autonomous coding agent is running in a CDE, the developer (or the platform) needs to know what it's doing, whether it's stuck, and whether it's doing something dangerous. That requires instrumentation at the CDE layer, not just the agent layer.

- **"How do you think about developer experience for platform tools?"** -- The bar is: a developer should be able to start using the tool without reading documentation. bilrost is `bilrost up` and you have a sandbox. The MCP server is one config block and you have 29 tools. Framework adapters are one import swap. If the getting-started experience requires more than one step, the abstraction is wrong. For Coder's AI tools, the same principle applies: enabling an autonomous agent in a CDE should be a toggle, not a migration.

- **"What interests you about Coder specifically?"** -- Two things. First, the problem is real and specific: autonomous coding agents need sandboxed environments with controlled resource access, and CDEs are the natural hosting primitive. Second, Coder is open-source at scale — 100k+ stars means engineering decisions have immediate consequences for real users. I want to work where the rigor matters.

## Cover Letter Hooks
- I built a hardened VM sandbox (bilrost) because autonomous agents need containment before they need capabilities — and Coder is solving the same problem at production scale for development environments hosting autonomous coding agents.
- The integration surface is the hard part: my MCP server bridges Python and TypeScript over stdio transport (29 tool calls in 3.94s), and my 7 framework adapters each implement the framework's native interface so users swap one import. Coder's AI Tools team faces the same challenge — making CDEs work for every agent framework without forcing each one to build a custom integration.
- I publish on PyPI, run CI against latest upstream versions with zero-skip policy, and break the build the same day a dependency ships a breaking change. That's the engineering discipline a 100k-star open-source project requires.
- My agent infrastructure work covers the full stack Coder's AI Tools team needs: sandboxed execution (bilrost), agent runtime (Vindler), observability (qortex-observe), and cross-framework integration (MCP + 7 adapters).

## Application Notes
- Coder is open-source-first. Emphasize the open-source engineering practices: PyPI, CI conformance, documentation sites, upstream test suite compatibility.
- The JD says "defining how AI agents operate in development workflows" — map this directly to the framework adapter and MCP experience. The candidate has already defined agent integration surfaces.
- Go is the primary gap. Address it proactively in the application, not defensively. The trajectory (Python -> TypeScript -> Rust -> Go) is natural for someone moving toward systems languages.
- Emphasize REMOTE availability — no relocation friction, available across US/Canada time zones.
- Coder's 100k+ GitHub stars is a signal they care about community and ecosystem. Mention PyPI downloads, documentation quality, and CI practices — not just features.
- If there's a free-text field, lead with the bilrost/sandbox story. Coder's core product is environments, and bilrost is an environment purpose-built for agent containment. The parallel is the strongest hook.
- The MCP angle is timely — MCP is becoming the standard protocol for agent-tool communication, and the candidate has production MCP experience. Call this out explicitly.
