# Anthropic — Software Engineer, Platform (Toolbox/MCP)

## Role Summary
- **Link**: https://job-boards.greenhouse.io/anthropic/jobs/4955107008
- **Location**: NYC, San Francisco, or Seattle (hybrid, 25% in-office)
- **Comp**: $320,000 - $405,000
- **Fit Score**: 5/5

## Lead Profile
Lead with **Developer Tools / DevEx**, blended with **Infrastructure**. The Toolbox team builds "infrastructure that enables Claude to safely interact with the outside world through the use of tools" — this is MCP infrastructure, tool execution environments, and the platform layer connecting Claude to external services. The candidate builds MCP servers and framework adapters as a core activity, not a side project. The infrastructure angle matters for the sandbox and isolation work, but the primary story is: I build the protocol layer that connects agents to the world.

## Tailored Pitch (2-3 sentences)
I've been building on both sides of MCP: a cross-language stdio transport serving 29 tool calls in 3.94s, and 7 framework adapters that implement native interfaces for CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, and LangChain.js — all passing framework-authored test suites. On the containment side, bilrost provides defense-in-depth agent sandboxing with per-tool network isolation, published on PyPI and deployable from a single CLI command. I want to build the platform that makes this safe connectivity the default, not something each team has to solve independently.

## Resume Emphasis
1. **MCP server** — Cross-language stdio transport bridging Python and TypeScript ecosystems. 29 tool calls in 3.94s with ~400ms server spawn. Direct experience with the protocol this team owns.
2. **Framework adapters** — 7 agent frameworks, each implementing the framework's native interface. 100+ CI tests with latest-version pinning and zero-skip policy. Demonstrates building composable platform components serving multiple consumers.
3. **bilrost** — Hardened VM sandbox for agent containment. Lima VM + OverlayFS + Docker dual-container isolation + gated sync. STRIDE threat model. PyPI published. Single CLI command. This is "secure, isolated execution environments" from the JD.
4. **qortex distributed service** — Active work on REST/GraphQL API layer with configurable storage backends. Service-oriented architecture for the retrieval engine.
5. **Vindler (openclaw)** — Agent runtime with OTel instrumentation. 90 merged PRs. Production system integration across sandbox, runtime, and observability layers.
6. **edX** — Education platform engineering at scale. Experience with large codebases, cross-team collaboration, and shipping product to millions of users.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| 6+ years distributed systems / backend engineering | MCP server (cross-language transport), qortex distributed service (REST/GraphQL), bilrost (VM provisioning + Docker isolation), edX platform. Agent runtime (Vindler) with 90 merged PRs. | strong |
| Service-oriented architecture | qortex: retrieval engine with pluggable storage backends, adapter pattern for 7 frameworks. MCP: service boundary between agent and tools via stdio transport. bilrost: dual-container architecture with per-tool network policy. | strong |
| 0-to-1 product building | Every project is greenfield. qortex, bilrost, Vindler, buildlog, MCP server — all designed, built, and shipped from scratch. | strong |
| Fast-moving environment, navigating ambiguity | Built across 6+ projects simultaneously. buildlog: 16K lines, realized wrong approach, pivoted. Framework adapters: maintained against 7 upstream frameworks with breaking-change-same-day CI. | strong |
| Full ownership mentality | PyPI published packages (qortex, bilrost). End-to-end: design, implement, test, document, ship, maintain. STRIDE threat model is self-initiated, not assigned. | strong |
| Build vs. buy decisions | MCP stdio transport: built because no cross-language solution existed for our architecture. Chose Lima over raw QEMU for bilrost (simpler UX, sufficient isolation). Chose OTel over custom telemetry (standard wins). | strong |
| REST API experience | qortex distributed service: REST/GraphQL API layer in active development. MCP server exposes tool schemas over JSON-RPC. | moderate |
| NLP and ML model understanding | Thompson Sampling, Beta-Bernoulli posteriors, PPR on knowledge graphs. Linguistics background (Classical Latin, Old Norse). Not ML training, but principled ML application. | moderate |

## Why This Role (authentic)
I use MCP every day — not as an abstraction, but as a transport protocol I've implemented and debugged. When I built the qortex MCP server, I hit the real problems: stdio buffering across language boundaries, tool schema validation, server lifecycle management. The Toolbox team owns the infrastructure that makes Claude's tool use work, which means the problems are the same ones I've been solving, except at Anthropic scale with Anthropic constraints (security, reliability, latency budgets that matter). What's compelling is the mission beneath the platform: safe connectivity between Claude and the outside world. bilrost exists because I believe agent-tool interaction needs containment by default. This team is building that default for everyone.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No experience at Anthropic's production scale | All current systems are single-developer, single-node. The patterns are sound (service boundaries, adapter pattern, transport abstraction), but haven't been tested at millions-of-requests scale. Frame as: the architectural instincts are there, the scale experience will come from the team. |
| Rust is "learning," not "proficient" | Anthropic likely uses Rust for performance-critical platform components. Currently strong Python/TypeScript, actively learning Rust. Trajectory matters: the candidate learns new languages by building things (Interlinear: Icelandic NLP, framework adapters: 7 different APIs). |
| No frontend/React experience listed | The JD lists React as preferred. This is genuinely a gap. Frame as: the candidate builds CLI tools and APIs, not UIs. If the role requires dashboard work, the ramp will be real but bounded — the data layer and API design skills transfer. |
| No experience on a platform team at a company | All platform work is solo. No experience with platform team dynamics: on-call, SLA negotiation, internal customer management. edX is the closest analog for working in a large engineering org. |
| Current work is stdio-based MCP, not the full protocol surface | MCP has HTTP/SSE transport, authentication, and features beyond stdio. Frame as: deep on one transport, aware of the protocol surface, ready to go deep on the rest. |

## If They Ask X, Say Y
- **"Walk me through your MCP implementation."** → The qortex MCP server exposes retrieval, feedback, and graph exploration as tool calls over stdio transport. The transport layer bridges Python (engine) and TypeScript (client ecosystem) — JSON-RPC over stdin/stdout with newline-delimited framing. Server lifecycle is managed per-session: spawn, handshake, tool discovery, execution, teardown. 29 tool calls in 3.94s including ~400ms spawn overhead. The design tradeoff was simplicity over performance — stdio is synchronous, which is fine when the LLM call downstream is 500ms-5s, but wouldn't scale for high-throughput scenarios. That's exactly why I'd want to work on the infrastructure that solves this properly.

- **"Why Platform over Research?"** → Because the leverage is higher. A research breakthrough helps one team. A platform component that makes tool use secure and fast helps every team building on Claude. I've done both — the retrieval research (Thompson Sampling, PPR) and the platform work (MCP server, framework adapters, sandbox). The platform work compounds. When I ship an adapter, 7 frameworks gain the capability simultaneously. That's the kind of multiplication I want to do at Anthropic's scale.

- **"Describe a technically challenging system you built from scratch."** → bilrost. The threat model came first: agents running arbitrary code need containment that doesn't depend on the agent cooperating. So: Lima VM for OS-level isolation, OverlayFS so filesystem mutations don't persist unless explicitly synced, Docker dual-container inside the VM (one for the agent runtime, one for tool execution), UFW firewall rules that default-deny and allowlist per-tool. The gated sync layer uses gitleaks to scan before anything leaves the sandbox. Single command to provision: `bilrost up`. The hardest part was making it feel simple — the user runs one command, but underneath it's Ansible provisioning a VM with Docker, configuring network policies, and setting up the sync pipeline.

- **"Tell me about a time you had to make a build-vs-buy decision."** → For bilrost, the VM layer. Options: raw QEMU (maximum control, maximum complexity), Firecracker (fast boot, but overkill for dev sandboxes), Lima (macOS-native, good enough isolation, simple UX). Chose Lima because the containment model doesn't require microsecond boot times — it needs reliable isolation and developer ergonomics. The per-tool network isolation runs on UFW inside the VM, which is boring and correct. I'd rather have boring infrastructure that I can reason about than clever infrastructure that surprises me.

- **"How do you handle maintaining compatibility across multiple consumers?"** → The framework adapter pattern. Each adapter implements the framework's native interface — LangChain's BaseRetriever, CrewAI's Tool, Agno's toolkit pattern. The CI runs each framework's own test suite against our adapter, pinned to the latest released version with a zero-skip policy. If CrewAI ships a breaking change on Monday, our CI fails on Monday. This is deliberate: I'd rather have a red build than a silent incompatibility. At Anthropic, the consumers would be internal teams instead of open-source frameworks, but the pattern is identical — implement their interface, test against their expectations, break loudly when the contract changes.

- **"What's your experience with security and isolation?"** → STRIDE threat model for agent containment. I found that OpenClaw's identity layer was an attack surface — workspace files injected verbatim into system prompts. Rather than patch the injection point, I built containment: the sandbox can't exfiltrate data even if the agent is compromised, because network access is default-deny with per-tool allowlisting. 117 passing tests on the network isolation layer. The security philosophy is defense-in-depth: assume each layer will fail, make sure the next layer catches it.

## Cover Letter Hooks
- I've implemented MCP from both sides — a Python server exposing 29 tool calls over stdio transport, and framework adapters that let 7 agent ecosystems consume those tools through their native interfaces — so I know exactly where the protocol's current abstractions help and where they create friction.
- bilrost provisions a hardened agent sandbox from a single CLI command: Lima VM, Docker dual-container isolation, per-tool network policy, gated sync with secret scanning — because I believe the platform that connects Claude to the outside world needs containment as a default, not an afterthought.
- Maintaining 7 framework adapters against upstream breaking changes taught me what platform engineering actually requires: implement the consumer's interface, pass the consumer's tests, and break loudly the same day the contract changes.
- The thread connecting my work — MCP transport, framework adapters, agent sandbox, observability pipeline — is that I keep building the infrastructure layer between AI systems and the world. The Toolbox team is where that work belongs.

## Application Notes
- The JD lists multiple platform sub-teams (Accel, Multicloud, Auth, Billing, Toolbox). Make clear in the application that Toolbox is the target — mention MCP by name.
- Emphasize the "secure, isolated execution environments" language from the JD — bilrost maps directly.
- "Composable platform components serving multiple use cases" is the adapter pattern. Use this exact framing.
- The preferred qualifications include React — acknowledge this honestly as a gap if asked, don't pretend.
- If there's a location preference field, NYC is probably strongest for this candidate if relocation flexibility exists. Note the role offers NYC, SF, or Seattle.
- The "0-to-1 product building" requirement is the candidate's strongest differentiator. Every project is greenfield. Lead with this in any free-text field.
- Mention PyPI publishing — it signals shipping culture, not just building culture.
