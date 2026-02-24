# Anthropic — Research Engineer, AI Observability

## Role Summary
- **Link**: https://job-boards.greenhouse.io/anthropic/jobs/5125083008
- **Location**: San Francisco, CA (hybrid, 25% in-office)
- **Comp**: $320,000 - $405,000
- **Fit Score**: 5/5

## Lead Profile
Lead with **Research Engineer**, blended with **Infrastructure**. This role lives at the intersection of measurement and systems — the team builds AI-based monitoring to help Anthropic understand massive datasets and surface patterns in unstructured data. The observability stack (qortex-observe) and the retrieval quality measurement pipeline are direct analogs. The research angle matters because the team designs analytical frameworks, not just dashboards.

## Tailored Pitch (2-3 sentences)
I built the observability pipeline I'd want to use on this team: OpenTelemetry traces spanning query through retrieval through graph exploration through feedback, with Grafana dashboards tracking posterior drift and retrieval quality degradation in real time. The measurement discipline carries — qortex's benchmark suite produces precision, recall, and nDCG numbers against a controlled domain, not vibes. I want to apply that same rigor to monitoring Claude's behavior at scale.

## Resume Emphasis
1. **qortex-observe** — Full OTel instrumentation pipeline (Grafana, Jaeger, Prometheus) tracing agent behavior from query to retrieval to feedback. Direct analog to monitoring AI systems at scale.
2. **qortex retrieval evaluation** — Closed-loop measurement: +22% precision, +26% recall, +14% nDCG on a controlled 20-concept benchmark. Evidence of building evaluation frameworks, not just features.
3. **Thompson Sampling / posterior analysis** — 30 days of production posterior divergence data. Experience processing and analyzing large volumes of unstructured signal to surface unexpected patterns.
4. **Framework adapters** — 7 frameworks with 100+ CI tests. Demonstrates building usable tools across ecosystems with reliability and documentation emphasis.
5. **buildlog** — 16K-line developer workflow system with gauntlet review. Experience productionizing internal tools, plus an honest lesson about knowing when the approach is wrong.
6. **edX + linguistics** — Education platform at scale (large-scale data processing context) and philology background (systematic analysis of unstructured text is literally the training).

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| 5+ years SWE with ML exposure | Agent systems with Thompson Sampling, Beta-Bernoulli posteriors, PPR graph algorithms. Prior edX platform engineering. | strong |
| LLM application development (context engineering, evaluation, orchestration) | qortex is a context engineering system — retrieval + graph explore + feedback loop. 7 framework adapters handle orchestration. Evaluation: precision/recall/nDCG benchmark suite. | strong |
| Monitoring/observability systems | qortex-observe: OTel traces from query → retrieval → graph explore → feedback. Grafana dashboards, Jaeger traces, Prometheus metrics. | strong |
| Large-scale data processing | OTel instrumentation generating structured telemetry across agent workflows. edX content delivery at scale. Posterior analysis over 30 days of production data. | moderate |
| Productionizing internal tools | bilrost published on PyPI, single-command provisioning. buildlog: git hooks + gauntlet review integrated into developer workflow. MCP server: 29 tool calls in 3.94s. | strong |
| Context-switching infrastructure ↔ product thinking | Built both the OTel pipeline (infrastructure) and the retrieval quality dashboard (product). bilrost is infrastructure; framework adapters are product surface. | strong |
| AI safety / responsible deployment | bilrost STRIDE threat model for agent containment. Sandbox with per-tool network isolation. The observability work is motivated by making agent behavior auditable. | moderate |
| Comfort with ambiguity in small, growing teams | Every project is 0-to-1. qortex, bilrost, Vindler — all built from scratch without a playbook. buildlog was 16K lines exploring the wrong abstraction, and that's fine. | strong |

## Why This Role (authentic)
The AI Observability team is building the thing I've been building for myself: instrumentation that makes agent behavior legible. When I built qortex-observe, it was because I needed to answer "is the retrieval getting better or worse?" in real time — posterior drift on a dashboard instead of running a benchmark suite manually. Anthropic is the right place because the scale is different: monitoring Claude's behavior across deployments is the version of this problem that actually matters. The team description mentions "autonomous investigation and action on findings," which is exactly the agentic loop I've been studying — except applied to safety oversight instead of retrieval quality. That's a more important application.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No experience at Anthropic-scale data volumes | The instrumentation patterns transfer — OTel is OTel. The gap is operational experience with petabyte-scale pipelines. Trajectory: actively working on distributed qortex service (REST/GraphQL, configurable storage backends). |
| No formal AI safety research publications | The safety motivation is real but the credential is thin. Frame as: the work is safety-adjacent (agent containment, threat modeling, observability for auditability) even if not published in safety venues. |
| Python/TypeScript, not yet proficient in languages Anthropic may use internally | Strong Python, strong TypeScript, learning Rust. The learning trajectory is toward systems languages. Ansible experience shows comfort with operational tooling. |
| Team is "emerging" — may want someone with prior observability platform experience at a company | All observability work is self-directed, not on an observability team. Reframe: that means every design decision was mine, including the ones that were wrong. I know what I'd do differently. |

## If They Ask X, Say Y
- **"Walk me through your observability architecture."** → qortex-observe instruments four layers: query intake, vector retrieval, graph exploration (PPR + Thompson Sampling), and feedback recording. Each produces OTel spans with structured attributes — retrieval scores, edge weights, posterior parameters. Grafana dashboards show retrieval latency distributions and posterior drift over time. Jaeger gives per-query trace waterfall. The design choice was to make every signal a first-class span, not a log line, because spans compose and logs don't.

- **"Why Anthropic? Why not stay independent?"** → Because the problem I care about — making AI systems legible and auditable — requires data I can't generate alone. Monitoring one agent's retrieval quality taught me the patterns. Monitoring Claude across millions of conversations is where those patterns actually produce safety outcomes. I want to work where the instrumentation matters most.

- **"What's the hardest technical challenge you've faced?"** → Composing three retrieval signals (vector similarity, PPR, Thompson Sampling) into a single ranking without any of them dominating. The math is straightforward individually, but the weighting is an open design question. I solved it empirically: benchmark suite, controlled domain, ablation tests. The +22% precision came from getting the composition right, not from any one signal being better.

- **"Tell me about a failure and what you learned."** → buildlog. 16K lines of Python building an LLM-as-judge review system. The gauntlet review layer works — rule-based, fast, useful. But the core hypothesis was wrong: the problem isn't "have an LLM review code," it's "select the right context for the right situation," which is a bandit problem. I scrapped the wrong part and kept what worked. The lesson: if your evaluation says the system works but you keep wanting to override it, the evaluation is measuring the wrong thing.

- **"How do you work with researchers and cross-functional teams?"** → On Vindler, I worked across the agent runtime, sandbox, and observability layers — each with different constraints. The researchers care about retrieval quality, the platform team cares about latency, the safety team cares about auditability. OTel is the common language: everyone reads traces. I build the instrumentation so each team can ask their own questions without needing to understand the others' stack.

- **"How do you think about scaling human oversight of AI?"** → You need three things: instrumentation that captures the right signals, aggregation that surfaces anomalies without drowning humans in noise, and interfaces that let investigators drill down when something looks wrong. qortex-observe does the first. The Anthropic AI Observability team is building all three at the scale that matters. My instinct is that the "surfacing anomalies" layer is where the hardest unsolved problems live — that's where retrieval quality measurement and posterior drift detection become directly applicable.

## Cover Letter Hooks
- When I built OpenTelemetry traces spanning every layer of an agent's retrieval pipeline — from query intake through graph exploration through feedback — I was solving the same problem your AI Observability team exists to solve: making AI behavior legible enough for humans to oversee.
- The numbers tell a story about measurement discipline: +22% precision, +26% recall, +14% nDCG — not from a better embedding model, but from a feedback loop with Thompson Sampling on knowledge graph edges, validated on a controlled benchmark with distractors.
- I built buildlog (16K lines), realized it was solving the wrong problem, scrapped the core, and kept the piece that worked — which taught me more about evaluation design than anything I shipped successfully.
- My background in philology — systematic analysis of Latin and Old Norse texts — is surprisingly relevant to a team that processes massive volumes of unstructured text to surface unexpected patterns. Corpus analysis is corpus analysis.

## Application Notes
- This role is on an emerging team. Emphasize 0-to-1 experience and comfort with ambiguity in the application form.
- The JD stresses "context engineering, evaluation, orchestration" — use those exact terms when describing qortex. Context engineering = retrieval + graph explore. Evaluation = benchmark suite. Orchestration = framework adapters.
- Mention "human oversight of AI systems" explicitly — it's the team's north star.
- The "agentic integrations enabling autonomous investigation" line maps directly to the agent runtime work (Vindler/openclaw). Call this out.
- If there's a free-text field, lead with the qortex-observe story, not the retrieval quality story. This role is about monitoring, not about the thing being monitored.
