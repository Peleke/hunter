# LiveKit — Senior Software Engineer, Agent Platform

## Role Summary
- **Link**: https://livekit.io/careers (HN Who's Hiring, February 2026)
- **Location**: Remote
- **Comp**: $120,000 - $250,000
- **Fit Score**: 5/5

## Lead Profile
Lead with **Infrastructure**, reinforced by **Research Engineer**. LiveKit's agent platform is the hosting and lifecycle layer for autonomous voice/video AI agents — containerization, distributed computing, networking. The candidate is independently building the same kind of system: bilrost (hardened VM sandbox for agent containment), Vindler (agent runtime with production loop), and qortex-observe (OTel instrumentation for agent behavior). The research angle matters because LiveKit isn't just hosting containers — they're defining how agents run safely at scale, which requires principled thinking about containment, resource isolation, and observability.

## Tailored Pitch (2-3 sentences)
I've been building the agent infrastructure stack LiveKit is productionizing: a hardened VM sandbox (bilrost) for agent containment with per-tool network isolation, a production agent runtime (Vindler) with lifecycle management, and full OpenTelemetry instrumentation tracing agent behavior from invocation through execution through feedback. The containment and observability aren't afterthoughts — they're the architecture. LiveKit's agent platform needs the same defense-in-depth thinking I've already applied: agents that are safe by default, observable by design, and isolated per-workload.

## Resume Emphasis
1. **bilrost** — Lima VM sandbox with per-tool network isolation, OverlayFS containment, gated sync, and STRIDE threat model. Single-command provisioning (`bilrost up`). Published on PyPI. This is the direct analog to LiveKit's agent hosting: safe containment for autonomous code execution.
2. **Vindler (openclaw)** — Production agent runtime with OTel instrumentation, 90 merged PRs. The operating system layer for multi-agent coordination. Maps to LiveKit's agent lifecycle management: how agents start, run, communicate, and terminate.
3. **qortex-observe** — Full OTel pipeline (Grafana, Jaeger, Prometheus) tracing from query through retrieval through graph explore through feedback. Monitoring agents in production is the core capability LiveKit's platform needs to expose to customers.
4. **MCP server** — Cross-language stdio transport, Python-TypeScript bridge, 29 tool calls in 3.94s. Demonstrates building the communication layer between agent runtimes and external services — the integration surface LiveKit's agents need.
5. **Framework adapters** — 7 frameworks, 100+ CI tests, all passing framework-authored test suites. Evidence of building platform-level abstractions that work across diverse agent runtimes.
6. **qortex** — Thompson Sampling on knowledge graph edges, real-time learning from feedback. Demonstrates the depth of agent systems thinking — not just hosting agents, but understanding what agents need from their platform.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| Agent platform infrastructure | bilrost (VM sandbox), Vindler (agent runtime), qortex-observe (instrumentation). Three layers of agent infrastructure built from scratch. | strong |
| Containerization and isolation | bilrost: Lima VM + OverlayFS + Docker dual-container isolation. Air-gapped by default, bridge only per-tool. STRIDE threat model with 7 analysis documents, 5 PoC exploits. | strong |
| Distributed computing / networking | MCP stdio transport bridging Python and TypeScript. Per-tool network isolation via UFW firewall rules. Distributed qortex service (REST/GraphQL) in active development. | moderate |
| Real-time systems | qortex graph explore: 0.02ms median latency. Feedback recording: <0.01ms. The performance discipline for real-time agent hosting is demonstrated. | moderate |
| Python and Rust | Strong Python (primary language across all projects). Actively learning Rust, planning a Rust runtime to replace the current Python layer. Natural trajectory toward LiveKit's systems language needs. | moderate |
| Production agent systems | Vindler: 90 merged PRs, production loop, OTel instrumentation. Not a prototype — a running system with real telemetry data and 30 days of posterior divergence evidence. | strong |
| Observability and monitoring | qortex-observe: OTel traces spanning full agent lifecycle. Grafana dashboards for latency, feedback rates, posterior drift. Jaeger for per-invocation trace waterfall. | strong |
| Security thinking | STRIDE threat model for agent containment. Found that workspace files injected into system prompts create an attack surface. Built per-tool network policies instead of patching. Defense-in-depth by design. | strong |

## Why This Role (authentic)
LiveKit is building the hosting platform for the kind of agents I've been building the containment and observability layer for. The parallel isn't abstract — bilrost exists because I needed to answer "how do you safely run autonomous code that makes its own tool calls?" and LiveKit is answering the same question for voice/video AI at production scale. The real-time angle is what makes this compelling: voice agents can't tolerate the latency budgets that text agents can, which means the platform layer has to be faster and the isolation has to be lighter. That constraint makes the engineering harder and more interesting. I want to work on agent infrastructure where the performance requirements are real, not hypothetical.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No Go experience (LiveKit is a Go shop) | Strong Python, strong TypeScript, actively learning Rust. Go is the natural next addition — syntactically simpler than Rust, and the concurrency model (goroutines) maps to the concurrent agent workload management I've already built in Python with async. Trajectory is toward systems languages. |
| No real-time voice/video experience | The containment and observability patterns are domain-agnostic. The gap is protocol-specific knowledge (WebRTC, SIP). Frame as: I bring the agent platform thinking, and the real-time media layer is learnable domain knowledge. |
| No production distributed systems at scale | All infrastructure work is single-node. Active work on distributed qortex service (REST/GraphQL, configurable storage backends). The architecture patterns transfer; the operational experience at LiveKit-scale is what I'd gain. |
| Work is all self-directed, not on a platform team | Every design decision was mine — including the wrong ones. Vindler's 90 merged PRs show sustained execution. The gap is collaborating on shared platform infrastructure, which is why joining a team is the right next step. |

## If They Ask X, Say Y
- **"Walk us through your agent infrastructure."** -- Three layers. bilrost is the containment layer: Lima VM with OverlayFS, Docker dual-container isolation, per-tool network policies via UFW. Vindler is the runtime layer: production loop, tool dispatch, lifecycle management. qortex-observe is the instrumentation layer: OTel spans on every agent action, Grafana dashboards, Jaeger traces. Each layer was built because the previous one needed it — you can't observe what you can't contain, and you can't contain what you can't run.

- **"Why LiveKit? Why agent platform specifically?"** -- Because I've been building this independently and I know the hard problems aren't the ones I've solved yet. Single-node containment works. What I haven't done is multi-tenant isolation at scale, or agent lifecycle management under real-time latency constraints, or resource scheduling across thousands of concurrent agent sessions. LiveKit is where those problems are real.

- **"You don't know Go. How do you plan to ramp up?"** -- I work across Python and TypeScript daily, with Rust as the active learning language. Go is syntactically simpler than Rust and the concurrency model — goroutines and channels — maps directly to the async agent workload patterns I've already built. I'd expect to be writing production Go within the first month. The harder ramp is LiveKit's real-time media stack, not the language.

- **"Tell us about your containment/security work."** -- bilrost started from a STRIDE threat model — I enumerated the attack surfaces of running autonomous agents (network exfiltration, filesystem escape, resource exhaustion, prompt injection via workspace files) and built walls for each. Lima VM provides the outer boundary. OverlayFS prevents persistent filesystem modification. UFW rules give each tool its own network policy. Gated sync means nothing leaves the sandbox without explicit approval. Five PoC exploits validated the threat model before I wrote the containment code.

- **"How do you think about agent lifecycle management?"** -- An agent needs to start (provision resources, load context), run (dispatch tools, handle errors, maintain state), and terminate (release resources, persist results, report telemetry). Vindler handles this as a production loop. The interesting design question is failure modes: what happens when an agent hangs, when a tool call times out, when the agent wants to spawn sub-agents? Those are the questions that make a hosting platform hard.

- **"What's the hardest technical problem you've solved?"** -- Composing three retrieval signals — vector similarity, Personalized PageRank, and Thompson Sampling — into a single ranking that's better than any individual signal. The math for each is well-studied, but the composition is an open design question. I solved it empirically: controlled benchmark, ablation tests, convergence analysis. The +22% precision came from getting the weighting right. The lesson for platform work: the hard problems are usually in the interfaces between well-understood components, not in the components themselves.

- **"How do you approach observability for agent systems?"** -- Every agent action becomes an OTel span with structured attributes — not a log line, because spans compose and logs don't. Retrieval scores, edge weights, latency breakdowns, tool call results all live as span attributes. Grafana shows aggregates (P50/P99 latency, feedback rates). Jaeger shows individual traces. The principle is: instrument first, decide what to dashboard later. You can always drop spans you don't need; you can't retroactively add spans you didn't capture.

## Cover Letter Hooks
- I've been building a parallel version of LiveKit's agent platform independently: a hardened VM sandbox for agent containment (bilrost), a production runtime for agent lifecycle management (Vindler), and full OTel instrumentation tracing agent behavior from invocation through feedback (qortex-observe).
- When I needed to answer "how do you safely run autonomous code that makes its own tool calls?", I started with a STRIDE threat model, wrote five proof-of-concept exploits, and then built the containment — Lima VM, per-tool network isolation, gated sync, OverlayFS. The same question, at production scale with real-time latency constraints, is what LiveKit's agent platform team is solving.
- The numbers from my agent infrastructure work: 0.02ms median graph explore latency, 29 MCP tool calls in 3.94s, 117 passing tests on the network isolation layer. I build agent systems with the same performance discipline that real-time voice/video demands.
- I don't just build agents — I build the infrastructure that makes agents safe and observable. That distinction is why LiveKit's agent platform role is the one I want.

## Application Notes
- LiveKit is a Go shop. Acknowledge this directly and frame the trajectory: Python -> TypeScript -> Rust -> Go is a natural systems language progression. The concurrent agent workload patterns already exist in the work.
- The JD mentions "containerization, distributed computing, networking" — map these directly to bilrost (containerization), distributed qortex service (distributed computing), and MCP stdio transport + per-tool network isolation (networking).
- Emphasize the REMOTE aspect as logistically clean — no relocation concerns, immediate start.
- The real-time voice/video angle is LiveKit's differentiator. Acknowledge it as the learning edge rather than pretending familiarity.
- "Building the software stack for agentic computing" is LiveKit's positioning. The candidate is doing exactly this independently — make the parallel explicit and specific, not vague.
