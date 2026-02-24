# LiveKit — Senior Software Engineer, Agent Platform — READY TO SUBMIT

## Job Posting Details

- **Title**: Senior Software Engineer, Agent Platform (also listed as "Agents Hosting")
- **Company**: LiveKit
- **Apply URL**: https://jobs.ashbyhq.com/livekit/f152aa9f-981c-4661-99d3-6837654b9c8b
- **Alt listing**: https://jobs.ashbyhq.com/livekit/1757f49e-7e19-4c45-85f7-e4637dff66fb (Senior Software Engineer, Agents)
- **Location**: Remote (US)
- **Comp**: $120,000 - $250,000
- **Tech stack**: Go, Firecracker, Docker, Temporal

### Responsibilities (from JD)
- Develop the core platform for hosting agents
- Build resilient systems to deploy agents across global data centers
- Create end-to-end tooling for seamless management of hosted agents
- Work with Go, Firecracker, Docker, Temporal, and similar technologies

### Desired Traits (from JD)
- Obsess with crafting code that is fast, reliable, and practical for the problem
- Known as the go-to person for tackling tough technical problems
- Work hard, build and ship fast
- Clearly explain complex technical concepts to others
- Fast learner who frequently picks up new languages and tools

---

## Cover Letter

Dear LiveKit Hiring Team,

I've been building an agent runtime, a hardened sandbox, and an observability layer independently — when I saw LiveKit's Agent Platform team, I realized you're productionizing the same architecture at scale.

Over the past year I've shipped three layers of agent infrastructure from scratch. **bilrost** is a Lima VM sandbox for agent containment: per-tool network isolation via UFW firewall rules, OverlayFS to prevent persistent filesystem modification, gated sync so nothing leaves the sandbox without explicit approval, and Docker dual-container isolation — air-gapped by default, bridged only per-tool. The design started from a STRIDE threat model with seven analysis documents and five proof-of-concept exploits, because I needed to answer the same question LiveKit's platform answers at scale: how do you safely run autonomous code that makes its own tool calls? bilrost is published on PyPI and provisions with a single command.

**Vindler** is the production agent runtime sitting inside that sandbox — lifecycle management, tool dispatch, and the production loop that coordinates agent execution. Ninety merged PRs, OpenTelemetry instrumentation on every action. **qortex-observe** completes the stack: OTel traces spanning from query intake through retrieval through graph exploration through feedback, with Grafana dashboards tracking latency distributions and posterior drift, and Jaeger for per-invocation trace waterfall. **cadence**, the newest layer, is a signal bus for ambient agency — event-driven coordination across agent workloads.

These map directly to what the Agent Platform team is building. bilrost's containment model — profile-based config, network isolation, defense-in-depth — is the same problem space as hosting agents in Firecracker microVMs across global data centers. Vindler's lifecycle management is agent orchestration. qortex-observe is the telemetry layer that makes hosted agents debuggable in production.

I want to be direct about a gap: Go is not yet in my production stack. Python and TypeScript are daily drivers, and Rust is the active learning language. But Go's concurrency model — goroutines and channels — maps directly to the async agent workload patterns I've already built, and its simplicity relative to Rust makes the ramp realistic. I'd expect to be writing production Go within the first month. The harder ramp is LiveKit's real-time media infrastructure, not the language.

LiveKit is building the hosting platform for the kind of agents I've been building the containment and observability layer for. The parallel isn't abstract — it's three shipped projects deep. I want to work where the performance requirements are real, not hypothetical, and where agent safety is an engineering constraint enforced at the platform level.

Peleke Sengstacke

---

## Application Form Notes

| Field | Value |
|-------|-------|
| **Name** | Peleke Sengstacke |
| **Portfolio** | https://peleke.dev |
| **GitHub** | https://github.com/Peleke |
| **Location** | Syracuse, NY (temporarily); home base Atlanta, GA |
| **Work Authorization** | US Citizen (no sponsorship needed) |
| **Availability** | Immediate |

### If there is a free-text "Why LiveKit?" or "Additional Info" field:

> I've been independently building the same stack LiveKit's Agent Platform team is productionizing: a hardened VM sandbox for agent containment (bilrost — STRIDE threat model, per-tool network isolation, published on PyPI), a production agent runtime with lifecycle management (Vindler — 90 merged PRs, OTel instrumentation), and full observability tracing agent behavior from invocation through feedback (qortex-observe — Grafana, Jaeger, Prometheus). The containment and observability aren't afterthoughts — they're the architecture. I want to bring that defense-in-depth thinking to LiveKit's agent hosting at production scale.

---

## Key Links to Include

| What | URL |
|------|-----|
| Portfolio | https://peleke.dev |
| GitHub | https://github.com/Peleke |
| bilrost (sandbox) | https://pypi.org/project/bilrost/ |
| qortex (retrieval + observability) | https://pypi.org/project/qortex/ |
| LiveKit Agent Platform JD | https://jobs.ashbyhq.com/livekit/f152aa9f-981c-4661-99d3-6837654b9c8b |
| LiveKit Agents JD | https://jobs.ashbyhq.com/livekit/1757f49e-7e19-4c45-85f7-e4637dff66fb |

---

## Interview Prep Quick Sheet

### 30-Second Pitch

"I've spent the past year building agent infrastructure from scratch — a hardened VM sandbox for agent containment, a production runtime for agent lifecycle management, and full OpenTelemetry instrumentation tracing agent behavior end-to-end. When I saw LiveKit's Agent Platform role, the alignment was immediate: you're productionizing the same architecture — containment, orchestration, observability — at the scale where it actually matters. I bring the systems thinking and security discipline. LiveKit brings the real-time constraints and production traffic that make the engineering harder and more interesting. Go is the one gap, and the ramp is realistic given my trajectory through Python, TypeScript, and Rust."

### 5 Most Likely Questions and Answers

**1. "Walk us through your agent infrastructure. How do the pieces fit together?"**

Three layers, each built because the previous one needed it. bilrost is the containment layer: Lima VM provides the outer boundary, OverlayFS prevents persistent filesystem modification, Docker dual-container isolation gives each agent workload its own environment — air-gapped by default, bridged only per-tool via UFW firewall rules. Gated sync means nothing leaves the sandbox without explicit approval. The design started from a STRIDE threat model — I wrote five PoC exploits before I wrote the containment code.

Vindler sits inside that sandbox as the runtime layer: production loop, tool dispatch, lifecycle management. Ninety merged PRs, not a prototype. qortex-observe is the instrumentation layer: OTel spans on every agent action — retrieval scores, edge weights, latency breakdowns, tool call results — all as structured span attributes, not log lines. Grafana shows aggregates, Jaeger shows individual traces. Each layer was built because the previous one needed it — you can't observe what you can't contain, and you can't contain what you can't run.

**2. "You don't know Go. LiveKit is a Go shop. How do you plan to ramp up?"**

I work across Python and TypeScript daily, and Rust is the active learning language. Go is syntactically simpler than Rust, and the concurrency model — goroutines and channels — maps directly to the async agent workload patterns I've already built. The concurrency primitives are more explicit than Python's asyncio, which I'd consider an advantage for the kind of concurrent agent session management LiveKit needs. I'd expect to be writing production Go within the first month. The harder ramp is LiveKit's real-time media stack and the specifics of your Firecracker integration, not the language. I'm a fast learner who picks up new languages and tools routinely — that's how I work.

**3. "How do you think about deploying agents safely across global data centers?"**

Safety at scale requires defense-in-depth, not a single isolation boundary. At the infrastructure level, each agent workload needs its own resource boundary — that's what Firecracker microVMs give you. Inside that boundary, you need filesystem isolation so one agent's state can't leak to another. Network isolation so agents can only reach the services they're authorized to use. And resource limits so a runaway agent can't starve its neighbors.

bilrost implements this pattern at single-node scale: Lima VM as the outer boundary, OverlayFS for filesystem isolation, per-tool UFW rules for network isolation, gated sync for data egress control. The architecture translates to multi-node — the isolation primitives change (Firecracker instead of Lima, Temporal workflows instead of local orchestration), but the defense-in-depth principle is the same. The piece I haven't built yet is the scheduling layer: deciding which data center, which node, which resource pool. That's the scale problem I'd be solving at LiveKit.

**4. "Tell us about a hard technical problem you solved and how you approached it."**

Composing three retrieval signals — vector similarity, Personalized PageRank, and Thompson Sampling — into a single ranking that's better than any individual signal. The math for each is well-studied, but the composition is an open design question. Each signal has different units, different distributions, different failure modes.

I solved it empirically: controlled benchmark with a 20-concept domain and distractors, ablation tests isolating each signal's contribution, convergence analysis over 30 days of posterior data. The result was +22% precision, +26% recall, +14% nDCG versus vanilla cosine similarity. The lesson that transfers to platform work: the hard problems are usually in the interfaces between well-understood components, not in the components themselves. Agent hosting is the same — Firecracker, Docker, Temporal are all proven. The difficulty is in composing them into a platform that's fast, reliable, and safe simultaneously.

**5. "How do you approach observability for agent systems? What would you instrument?"**

Every agent action becomes an OTel span with structured attributes — not a log line, because spans compose and logs don't. For an agent hosting platform, I'd instrument: session lifecycle events (start, tool call, completion, termination), resource utilization per session (CPU, memory, network), latency breakdowns at every boundary crossing (agent to tool, agent to model, agent to storage), error rates and failure modes by category, and tenant-level aggregates for capacity planning.

The principle I follow: instrument first, decide what to dashboard later. You can always drop spans you don't need; you can't retroactively add spans you didn't capture. In qortex-observe, this approach meant I caught posterior drift I wasn't looking for — the instrumentation surfaced a pattern that no dashboard spec would have anticipated. For a hosting platform, that kind of emergent visibility is how you catch issues before customers report them.

### One Question to Ask Them

"How do you think about the isolation boundary between agent workloads today — is it primarily at the Firecracker VM level, or do you have additional layers inside the VM? I'm curious whether the threat model assumes agents are mutually untrusted or just untrusted relative to the host."

This question signals: familiarity with defense-in-depth thinking, understanding that isolation is a spectrum (not binary), and genuine curiosity about LiveKit's specific architecture decisions. It also opens a conversation about bilrost's multi-layer isolation model.

---

## Location Note

This is a remote role. Candidate is currently in Syracuse, NY (temporary) with home base in Atlanta, GA. US citizen, no sponsorship required, available immediately. No relocation concerns.

---

## Pre-Submission Checklist

- [ ] Submit via Ashby: https://jobs.ashbyhq.com/livekit/f152aa9f-981c-4661-99d3-6837654b9c8b
- [ ] Also consider applying to "Senior Software Engineer, Agents": https://jobs.ashbyhq.com/livekit/1757f49e-7e19-4c45-85f7-e4637dff66fb
- [ ] Attach resume (infrastructure-weighted version if available)
- [ ] Include portfolio link: https://peleke.dev
- [ ] Include GitHub link: https://github.com/Peleke
- [ ] Paste cover letter into any free-text field
- [ ] If separate "Why this role?" field exists, use the short-form pitch from Application Form Notes above
