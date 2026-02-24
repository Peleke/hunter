# TensorZero — Founding Member of Technical Staff

## Company Context

TensorZero is building an open-source LLM gateway and "automated AI engineer" (Autopilot) that uses contextual bandits — Thompson Sampling, Track-and-Stop for best-arm identification — to route and optimize LLM inference. The mathematical core of their product is the same math I have been independently building into qortex: Beta-Bernoulli posteriors, Thompson Sampling on weighted edges, posterior divergence as evidence of learning. The team is extraordinary — Viraj Mehta (CMU ML PhD, data-efficient RL), Aaron Hill (Rust compiler maintainer), Andrew Jesson (Oxford PhD, Bayesian ML, causal inference, 4k+ citations), Alan Mishler (CMU Stats PhD, sequential decision making, 1.2k+ citations), Shuyang Li (Staff SWE at Google Search). This is a seed-stage company ($7.3M raised, FirstMark / Bessemer / Bedrock) with years of runway, a Rust-first codebase, and a "no distinction between engineers and researchers" culture that matches exactly how I work.

## Role Summary (Rust MTS — Back-end Engineering / Systems)
- **Link**: https://jobs.ashbyhq.com/tensorzero/6cf3673e-5377-4c22-9f0c-ebf27c8567b1
- **Fit Score**: 3.5/5
- **Key Requirements**: Strong systems SWE background, extensive experience with systems programming languages (Rust, C/C++, Zig), technical leadership, drive large projects from inception to deployment, in-person NYC
- **Honest Assessment**: Domain alignment is perfect. The Rust requirement is the gap. I am actively learning Rust and planning a Rust runtime layer, but I do not have "extensive experience" — Aaron Hill maintained the Rust compiler in college. That is the bar on this team.

## Role Summary (ML MTS — ML Engineering / Research)
- **Link**: https://jobs.ashbyhq.com/tensorzero/93f2c500-3520-4817-aaeb-311d3a012295
- **Fit Score**: 5/5
- **Key Requirements**: ML engineering or research background, advanced inference strategies (MCTS), optimization recipes (RL, APE), Bayesian methods
- **Honest Assessment**: This is the role. Thompson Sampling on knowledge graph edge weights, Beta-Bernoulli posteriors, convergence analysis, bandit-based optimization — I have been building exactly this. The posting may be delisted (no longer on Ashby public board as of Feb 2026), which means either it was filled or folded into general MTS hiring. Worth emailing directly to ask.

## Recommended Application Angle

**Apply to the ML MTS role first.** If it has been filled or delisted, apply to the Rust MTS role with a cover letter that leads entirely with the ML/bandit overlap and frames the systems work as the growth vector. The pitch is the same either way: "I have been independently building the same kind of system you are building, using the same math, at a different layer of the stack." For the Rust role, be direct that Rust is a growth area, not a current strength — but the mathematical alignment means I can contribute to the optimization and inference strategy layers from day one while ramping on systems work under Aaron Hill. The "hungry for personal growth" and "opportunity to expand your skill set" language in the Rust posting suggests they are open to candidates who bring complementary strengths and grow into the systems side.

If both roles are live, apply to both. The application will route to the same hiring team anyway. Let them decide where the fit is strongest.

## Lead Profile

**Research Engineer.** The one-liner: "Research engineer building instrumented agent systems that learn from feedback, with early evidence of principled agentic learning over context." This is the right frame because TensorZero's core product is a system that learns from feedback — they learn which LLM routes to take, I learn which retrieval paths to take. Same problem structure, same math, different application layer.

## Tailored Pitch (2-3 sentences)

I have been independently building the same kind of system TensorZero is building. qortex uses Thompson Sampling on knowledge graph edge weights with Beta-Bernoulli posteriors to make retrieval learn from usage feedback — the same math TensorZero uses for contextual bandits in LLM routing, applied at the retrieval layer instead of the inference layer. I have 30 days of production posterior divergence as evidence, controlled benchmarks (+22% precision, +26% recall, +14% nDCG vs vanilla cosine), and a published convergence analysis with interactive Beta distribution visualizations.

## Resume Emphasis

1. **qortex** — Thompson Sampling on edge weights, Beta-Bernoulli posteriors, PPR, posterior divergence tracking. Maps directly to TensorZero's contextual bandit core. Same math, different layer.
2. **Convergence analysis & measurement** — Closed-loop evaluation with precision/recall/nDCG on controlled domains. Interactive Beta distribution widget (Canvas API, Lanczos lnGamma). Demonstrates the "measure everything" discipline TensorZero needs.
3. **Framework adapters (7 frameworks, 100+ CI tests)** — Shows ability to build integrations at scale across diverse interfaces. Relevant to TensorZero's gateway needing to interface with every LLM provider.
4. **buildlog gauntlet** — Bandit-based reviewer selection. 16K lines, partially deprecated. The surviving insight (the problem is bandit selection over context) is directly relevant to TensorZero's approach.
5. **Interlinear** — Thompson Sampling for concept mastery tracking in adaptive language learning. Second independent application of bandits, different domain entirely.
6. **qortex-observe / OTel instrumentation** — Full observability stack (Grafana, Jaeger, Prometheus). Maps to TensorZero's observability layer.
7. **bilrost** — Systems engineering work: VM provisioning, network isolation, STRIDE threat modeling. Demonstrates the infrastructure instinct needed for gateway-level systems.
8. **MCP server / cross-language transport** — 29 tool calls in 3.94s, stdio transport. Relevant to the gateway's provider-agnostic interface layer.

## Proof Point Mapping

| Requirement | Evidence | Strength |
|-------------|----------|----------|
| Thompson Sampling / bandits | Beta-Bernoulli posteriors on KG edge weights, 30 days production posterior divergence, convergence analysis published | Strong — independent implementation of their core math |
| Bayesian ML understanding | Beta distributions, posterior updates, divergence tracking, interactive Beta widget with Lanczos lnGamma | Strong — built it, visualized it, published it |
| Optimization / feedback loops | qortex retrieval improves from usage: +22% precision, +26% recall, +14% nDCG vs vanilla cosine | Strong — measured closed-loop improvement |
| Systems engineering | bilrost VM sandbox (Lima, Ansible, Docker dual-container, STRIDE), MCP stdio transport, CI infrastructure | Moderate — real systems work, but Python/TS not Rust |
| Rust proficiency | Actively learning, planned runtime layer, not yet shipped production Rust | Weak — honest gap, addressed in Gap Analysis |
| Open-source experience | PyPI packages (qortex, bilrost), 7 framework adapters passing upstream test suites, mkdocs sites | Strong — building for the OSS ecosystem |
| Gateway / provider abstraction | 7 framework adapters with single-import swap, MCP cross-language transport | Strong — same pattern as multi-provider LLM gateway |
| Observability | Full OTel pipeline: Grafana dashboards, Jaeger traces, Prometheus metrics, per-retrieval instrumentation | Strong — directly maps to TensorZero's observability layer |
| A/B testing / experimentation | Bandit-based selection in buildlog gauntlet, Thompson Sampling exploration-exploitation | Moderate — implemented the math, not at production A/B scale |
| Evaluation rigor | Controlled 20-concept benchmark domain, precision@k/recall@k/nDCG@k, overhead profiling (0.02ms median graph explore) | Strong — principled evaluation, not vibes |
| Technical writing / communication | Published articles with convergence analysis, hand-drawn SVG diagrams, interactive widgets, "Peleke (ed: Claude)" byline | Strong — can explain the math clearly |
| Inception-to-deployment ownership | qortex (solo, full stack), bilrost (solo, published), Vindler (90 merged PRs), all self-directed | Strong — founding-role DNA |

## Why TensorZero (authentic)

The mathematical overlap is not a coincidence — it is convergent problem-solving. I started from "agents do not learn from their own usage" and independently arrived at Thompson Sampling on weighted edges with Beta-Bernoulli posteriors. TensorZero started from "LLM systems do not optimize themselves" and arrived at the same math for model routing. The fact that we reached the same family of solutions from different starting points suggests a shared understanding of how adaptive systems should work.

Working alongside Andrew Jesson and Alan Mishler on Bayesian ML is a genuine, specific learning opportunity I cannot get elsewhere. Jesson's work on causal inference and Bayesian deep learning, Mishler's work on sequential decision-making and uncertainty quantification — these are the people who can push my understanding of bandits from "practitioner who implements Thompson Sampling" to "researcher who knows when Thompson Sampling is the wrong choice and what to reach for instead." That is not flattery; that is an honest assessment of where my technical ceiling currently sits and where their expertise would raise it.

The "no distinction between engineers and researchers" philosophy is how I already work. I built the math and I built the infrastructure. I wrote the convergence analysis and I wrote the CI pipeline. I do not identify as "engineer" or "researcher" — I identify as "person who builds systems that learn and measures whether they actually do."

The stage matters too. This is a founding team building the core product. I want to build, not maintain. The open-source model means the work has to speak for itself, which is the accountability structure I prefer.

## Gap Analysis

| Gap | How to Frame |
|-----|-------------|
| Rust proficiency | Honest: I am actively learning Rust and have planned a Rust runtime layer for qortex, but I have not shipped production Rust. The team already has Aaron Hill, who maintained the Rust compiler. What I bring is the mathematical and ML layer — the optimization logic that the Rust gateway serves. I can ramp on Rust idioms faster than most because I understand the domain the Rust code is implementing. Trajectory: currently writing Rust for the planned runtime, expect working proficiency within 2-3 months of full-time immersion. |
| NYC relocation | Currently in Austin. This is a real logistical constraint, not a dealbreaker. I am willing to relocate for the right founding role. TensorZero is in Williamsburg. Timeline: would need 4-6 weeks for relocation logistics. Be upfront about this in the application — do not let it surface as a surprise. |
| No PhD | The ML researchers on the team (Jesson, Mishler, Mehta) all have PhDs. I do not. Frame: I arrived at the same mathematical tools independently, from an engineering direction. The work speaks — posterior divergence plots, controlled benchmarks, published convergence analysis. The ML MTS role asks for "ML engineering or research background," not a PhD specifically. |
| Production scale | qortex benchmarks are on a controlled 20-concept domain, not millions of inferences. TensorZero serves Fortune 50 enterprises. Frame: the evaluation methodology is rigorous regardless of scale. The math does not change. Infrastructure scaling (from bilrost/OTel work) demonstrates I can think about production systems. |
| No prior startup experience | edX is the closest — education at scale — but not a venture-backed startup. Frame: every project in the current portfolio was self-directed, solo-built, and taken from zero to published. That is the founding-team muscle. |

## If They Ask X, Say Y

**"Walk me through your Thompson Sampling implementation."**
qortex maintains Beta-Bernoulli posteriors on knowledge graph edge weights. Each edge starts with a Beta(1,1) prior — uniform, no opinion. When a retrieval path gets positive feedback, the alpha parameter increments; negative feedback increments beta. At query time, Thompson Sampling draws from each edge's posterior to decide which paths to explore, naturally balancing exploitation of known-good paths with exploration of uncertain ones. Personalized PageRank then propagates these sampled weights through the graph structure. The result: retrieval that learns which context components matter for which queries, with the exploration rate decreasing as evidence accumulates. I track posterior divergence over time as evidence that the system is actually learning — the distributions move away from the prior, and they move differently for different edge types.

**"How does your work relate to what we do at TensorZero?"**
Same math, different layer. You use Thompson Sampling and contextual bandits to decide which LLM model/prompt/strategy to route an inference to, optimizing for quality, cost, and latency. I use Thompson Sampling on knowledge graph edges to decide which retrieval paths to explore, optimizing for context relevance. Both are online learning problems where you update beliefs from feedback without retraining. Both use Beta-Bernoulli posteriors. Both face the exploration-exploitation tradeoff. The difference is where in the stack the bandit operates — you are upstream (model selection), I am downstream (context selection). A complete system probably needs both.

**"Your Rust experience seems limited. How would you contribute from day one?"**
Directly: the optimization and inference strategy layer is where my mathematical background maps. I can contribute to the bandit logic, evaluation pipelines, and experimentation framework immediately — those are language-agnostic algorithms that I have already implemented. The Rust learning curve is real but bounded: I understand the domain concepts the Rust code is expressing, which means I am learning syntax and idioms, not concepts. Aaron Hill's expertise is an accelerant — learning Rust from a compiler maintainer is a different trajectory than learning it from documentation alone. I expect working proficiency for production contributions within 8-12 weeks of full-time immersion.

**"Why leave Austin for NYC?"**
For the right founding role, relocation is straightforward. The specific draw is the team — working in person with Andrew Jesson and Alan Mishler on Bayesian ML, with Aaron Hill on Rust systems, is a concentration of expertise I cannot assemble remotely or find in Austin. The in-person requirement is a feature for a founding team, not a cost. I have no constraints that make NYC infeasible.

**"How do you think about the engineer vs. researcher distinction?"**
I do not make that distinction, which is why your "no distinction between engineers and researchers" line resonated. I built the Thompson Sampling implementation AND the CI pipeline. I wrote the convergence analysis AND the Ansible provisioning. The question is always "does the system actually learn, and can I prove it?" — that requires both the mathematical reasoning and the engineering to measure it. My buildlog project is instructive: 16K lines of code trying to make LLM-as-judge work for review, and the surviving insight was that the problem is bandit selection over context. The engineering taught me the research question.

**"What is the biggest technical bet you have made?"**
Betting that the right adaptive layer for agents is not fine-tuning or prompt optimization but online learning on the retrieval graph. Everyone else is trying to make the LLM smarter. I am trying to make the context smarter — give the LLM better inputs, and the outputs improve without touching the model. The bet is that Thompson Sampling on a knowledge graph is a more data-efficient and interpretable path to agent improvement than reinforcement learning from human feedback on the model itself. qortex's numbers (+22% precision, +26% recall) on a small controlled domain suggest the bet is directionally correct. TensorZero's success with bandits for model routing is evidence from the other side — the same class of solutions works at the inference layer too.

**"Tell me about working with PhDs and ML researchers."**
My background is in linguistics and philology — Classical Latin, Old Norse/Icelandic. That is a research discipline with its own rigor (textual criticism, morphosyntactic analysis, comparative method). I transitioned to engineering but kept the research instincts: form hypotheses, design controlled experiments, measure, publish. Working with Jesson and Mishler would be the first time I have peers who can challenge me on the mathematical foundations, which is precisely what I want. I know what I do not know — I can implement Thompson Sampling, but I may not know when Track-and-Stop is a better choice, or how to extend to contextual bandits with high-dimensional context. That is what I want to learn.

**"What do you know about our blog post on bandits / Track-and-Stop?"**
Track-and-Stop for best-arm identification is a different regime than what I have been working in. My implementation uses Thompson Sampling for exploration-exploitation in a non-stationary environment (retrieval preferences change). TensorZero's use of Track-and-Stop makes sense for the model routing problem because you want to identify the best arm (best model/prompt variant) with statistical confidence, then commit. The exploration budget matters more when each arm pull costs real money (LLM inference costs). I would be curious about how you handle non-stationarity — when a model provider updates their weights, your posteriors over that arm may become stale. That is a problem I have thought about in the retrieval context.

## The Mathematical Overlap Pitch

I have been independently building the same kind of system TensorZero is building, using the same math, at a different layer of the stack. qortex maintains Beta-Bernoulli posteriors on knowledge graph edge weights and uses Thompson Sampling to route retrieval through paths that have earned evidence of relevance — exactly how TensorZero uses contextual bandits to route LLM inferences through model/prompt configurations that have earned evidence of quality. I arrived at this approach not because I read your blog post, but because the exploration-exploitation tradeoff in retrieval has the same structure as the exploration-exploitation tradeoff in model selection: you have uncertain beliefs about which option is best, you update those beliefs from feedback, and you need to balance learning with performance. The convergence analysis I published — with interactive Beta distribution visualizations showing posterior divergence over 30 days — is the same kind of evidence your system generates when it identifies a winning arm. Same math. Same feedback loop. Same philosophy that AI systems should learn from their own usage rather than waiting for human retraining. The difference is where the bandit sits in the stack: you optimize the inference layer, I optimize the context layer. A founding team building both layers is the obvious next step.

## Application Notes

- **Primary target**: ML MTS role. Check if still active — it was not on the public Ashby board as of Feb 2026. Email the founders directly (Viraj Mehta or Gabriel Bianconi) with a brief note and the mathematical overlap pitch. LinkedIn warm outreach is also viable.
- **Secondary target**: Rust MTS role (still listed). Apply through Ashby with a cover letter that leads with the bandit/ML overlap and acknowledges Rust as the growth vector.
- **Apply to both if both are live.** Same team, same hiring pipeline. They will route to the right fit.
- **Timing**: The Rust MTS has been posted since Dec 2025. It is still open, which suggests they have not found the right person or are being selective. The ML role was posted and may have been filled or folded. Move quickly.
- **Equity consideration**: Seed stage, $7.3M raised. Equity at this stage is meaningful but illiquid. Salary range ($180-250K estimated) is below Anthropic-tier base but competitive for a seed company. The equity upside is the bet — if TensorZero succeeds, the founding equity is worth multiples of the salary delta. Evaluate against total comp at Anthropic or similar, not just base.
- **NYC logistics**: Budget 4-6 weeks for relocation from Austin. Williamsburg is a specific neighborhood — factor in housing costs (significantly higher than Austin). Be prepared to discuss timeline in the first conversation.
- **Open-source angle**: TensorZero's codebase is public. Before applying, read the Rust gateway code on GitHub (github.com/tensorzero/tensorzero). Understanding their implementation of the bandit logic will make the mathematical overlap pitch concrete rather than abstract. Reference specific code in the cover letter if possible.
- **The cover letter should be 4 paragraphs max**: (1) the mathematical overlap pitch, (2) what I have built and the evidence, (3) honest Rust assessment + growth trajectory, (4) why founding team + NYC.
