# Grafana Labs — Senior Software Engineer, GenAI & ML Evaluation Frameworks

## Role Summary
- **Link**: https://job-boards.greenhouse.io/grafanalabs/jobs/5559973004
- **Location**: USA Remote or Canada Remote
- **Comp**: $154,445 - $185,334
- **Fit Score**: 5/5

## Lead Profile
**ML / AI Engineer**, supplemented by **Developer Tools / DevEx.** This role sits at the intersection of AI evaluation and observability infrastructure. The ML profile provides the retrieval evaluation expertise (P@k, nDCG, golden test sets, regression tracking). The Dev Tools profile provides the CI/CD integration and framework packaging sensibility. The unique angle: the candidate already runs Grafana + Jaeger + Prometheus in production for AI system observability. This is the "we use your stack" story.

## Tailored Pitch (2-3 sentences)
I run Grafana, Jaeger, and Prometheus in production to observe AI retrieval systems — qortex-observe instruments the full pipeline from query through graph explore to feedback with OpenTelemetry. I evaluate retrieval quality with P@k, R@k, and nDCG@k on controlled domains, track posterior convergence over 30 days, and run 100+ CI tests across 7 framework adapters with a zero-skip policy. This role asks me to build evaluation frameworks for AI/ML systems on the observability platform I already use daily.

## Resume Emphasis
1. **qortex-observe — Grafana/Jaeger/Prometheus in production.** Full OTel instrumentation for AI retrieval. Traces span from query to retrieval to graph explore to feedback. Dashboards track retrieval latency, feedback rates, and posterior drift. This is not theoretical observability — it runs on their stack.
2. **Retrieval evaluation methodology.** P@5, R@5, nDCG@5 on a controlled 20-concept domain with distractors. Golden test set design. Before/after regression tracking (+22% precision, +26% recall, +14% nDCG). This maps directly to "golden test sets, regression tracking" in the job description.
3. **CI/CD integration of evaluation pipelines.** 100+ CI tests across 7 framework adapters. Zero-skip policy. Latest-version framework pinning — if a framework ships a breaking change, CI catches it the same day. This is evaluation-in-CI, which the role explicitly requires.
4. **Thompson Sampling convergence analysis.** 30 days of posterior divergence data. Convergence plots. Interactive Beta distribution widget. This is structured output evaluation and regression tracking for ML systems — the learning component of evaluation.
5. **Framework adapters as evaluation surface.** 7 adapters (CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, LangChain.js) each passing framework-authored tests. Each adapter is an evaluation boundary: does the integration still conform after framework updates?
6. **Open-source distribution.** PyPI packages, mkdocs docs, CI/CD pipelines. Grafana Labs is an open-source company. The candidate ships like one.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| Evaluation frameworks for AI/ML systems | Controlled retrieval benchmark suite: P@5, R@5, nDCG@5 on 20-concept domain with distractors. Overhead benchmarks with percentile reporting. Convergence analysis over 30 days. | Direct match |
| Prompt engineering and structured output evaluation | Three-signal retrieval composition (vector + PPR + Thompson Sampling) with structured evaluation at each stage. Interactive Beta distribution widget for visualizing posterior outputs. | Strong |
| Context-window management | Knowledge graph retrieval is fundamentally a context-window optimization problem — selecting the most relevant context for a given query. Thompson Sampling optimizes which context components to include. | Strong |
| Golden test sets | 20-concept controlled domain with known ground truth and distractors. Framework-authored test suites as golden conformance tests. | Direct match |
| Regression tracking | Before/after benchmarks: +22% precision, +26% recall, +14% nDCG. CI with latest-version pinning catches regressions same-day. Zero-skip policy. | Direct match |
| LLM-as-judge methods | Thompson Sampling on feedback is a principled alternative to LLM-as-judge — it uses Beta-Bernoulli posteriors instead of LLM scoring. Can articulate tradeoffs between both approaches. | Moderate — different approach to same problem |
| CI/CD integration of evaluation pipelines | 100+ CI tests, 7 framework adapters, latest-version pinning, zero-skip policy. Evaluation runs on every commit. | Direct match |
| Observability / Grafana experience | qortex-observe: Grafana + Jaeger + Prometheus with OTel instrumentation. Production dashboards tracking retrieval latency, feedback rates, posterior drift. Uses Grafana daily. | Direct match |

## Why This Role (authentic)
This is the role where every piece of my experience converges. I already run Grafana, Jaeger, and Prometheus to observe AI retrieval systems. I already evaluate retrieval quality with P@k and nDCG on controlled domains. I already integrate evaluation into CI/CD with 100+ tests and a zero-skip policy. The job description reads like a summary of what I built independently — evaluation frameworks, golden test sets, regression tracking, CI/CD integration — except Grafana Labs does it at scale, for the entire industry, on the observability platform I already use. And it's remote. Building GenAI evaluation frameworks on top of the Grafana stack, from home, is the exact role I'd design for myself.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No Go experience (Grafana's primary language) | Python and TypeScript are the languages of the GenAI ecosystem this role serves. Go for internal tooling can be learned; deep GenAI evaluation expertise in Python/TS cannot be quickly acquired. Currently learning Rust, which demonstrates systems-language learning appetite. |
| LLM-as-judge: different approach | Thompson Sampling on feedback is a principled alternative — Beta-Bernoulli posteriors instead of LLM scoring. Frame as complementary expertise: I bring the statistical evaluation perspective that makes LLM-as-judge results more rigorous. |
| No Grafana plugin/extension development experience | I'm a power user, not yet a contributor. But I understand the data model (traces, metrics, logs) from the producer side. Building evaluation dashboards as a user informs building evaluation frameworks as a developer. |
| Comp range ($154K-$185K) is lower than other targets | Remote USA/Canada is a significant quality-of-life benefit. Evaluate total comp including equity, benefits, and the remote premium. Grafana Labs is pre-IPO — equity could be significant. |

## If They Ask X, Say Y
1. **"Describe your experience with Grafana and observability."** → qortex-observe is a full OTel instrumentation layer for AI retrieval. I run Grafana for dashboards (retrieval latency, feedback rates, posterior drift), Jaeger for distributed traces (query to retrieval to graph explore to feedback), and Prometheus for metrics collection. This isn't a toy setup — it's how I monitor whether Thompson Sampling is actually improving retrieval quality over time. I chose the Grafana stack because it's the best open-source observability platform. Now I want to help build it.

2. **"How would you design an evaluation framework for GenAI systems?"** → Start with the evaluation taxonomy. Retrieval quality (P@k, nDCG) is different from response quality (LLM-as-judge) is different from system performance (latency, throughput). Each needs its own golden test set design, its own regression thresholds, its own CI integration strategy. The framework should make it easy to define golden sets, run evaluations on every commit, track regressions over time, and surface results in Grafana dashboards. The pipeline: define golden set, instrument the system with OTel, run evaluation suite in CI, push metrics to Prometheus, visualize in Grafana, alert on regressions.

3. **"How do you approach golden test set design?"** → Controlled domains with known ground truth. My retrieval benchmark uses a 20-concept authorization domain with distractors — concepts that are semantically similar but not relevant. I measure P@5, R@5, and nDCG@5, which catches both precision failures (wrong results) and ranking failures (right results in wrong order). The key insight: golden sets need distractors, not just positive examples. Without distractors, every retrieval system looks good.

4. **"What's your experience with CI/CD integration of evaluation?"** → 100+ CI tests across 7 framework adapters. Latest-version framework pinning — not locked versions, latest. Zero-skip policy: if a test is flaky, we fix it, not skip it. If CrewAI ships a breaking change on Tuesday, our CI fails on Tuesday. This is how you catch regressions at the speed frameworks ship. For GenAI evaluation in CI, the same philosophy applies: run evaluations on every commit, fail the build if quality regresses.

5. **"LLM-as-judge — what's your take?"** → It solves a real problem: evaluating open-ended generation where P@k doesn't apply. But it has calibration issues — different models judge differently, and the same model judges differently across prompts. I'd complement LLM-as-judge with statistical methods: inter-rater reliability metrics, confidence intervals, and where possible, closed-form evaluation like P@k and nDCG. Thompson Sampling is actually relevant here — you can model judge reliability as a bandit problem and weight judge scores by posterior confidence.

6. **"Why Grafana Labs over other observability companies?"** → Open source. Grafana Labs built the observability stack I chose independently — Grafana, Prometheus, and the broader LGTM stack. The GenAI evaluation role is the perfect convergence: I bring AI evaluation expertise to the observability platform I already run. And the remote-first culture means I can focus on the work.

7. **"Tell me about context-window management."** → Knowledge graph retrieval is fundamentally context-window optimization. Given a query, which chunks of context should fill the LLM's window? Thompson Sampling on edge weights learns which context components matter for which query patterns. PPR provides structural context (related concepts). Vector similarity provides semantic relevance. Composing these three signals produces a context window that's optimized for the query, not just semantically similar to it. This is measurable: +22% precision means 22% fewer irrelevant chunks in the context window.

## Cover Letter Hooks
1. "I run Grafana, Jaeger, and Prometheus in production to observe AI retrieval systems. qortex-observe instruments the full pipeline with OpenTelemetry — traces from query through graph explore to feedback, dashboards tracking posterior drift over 30 days. When I saw Grafana Labs hiring for GenAI evaluation frameworks, I recognized the job description as a summary of what I've already built on your stack."

2. "Evaluation without observability is guesswork. Observability without evaluation is noise. This role combines both, and that's exactly what I do: P@k and nDCG on controlled domains for evaluation, Grafana and Jaeger for observability, CI with 100+ tests and zero-skip policy for regression tracking. Building this as a platform feature for the entire Grafana ecosystem is the work I want to do."

3. "Most GenAI evaluation frameworks treat retrieval as a black box. I measure it at every stage: embedding latency, graph explore overhead (0.02ms median), context relevance (P@5), and learning convergence (30 days of posterior divergence). Bringing this evaluation methodology to Grafana Labs — the observability platform where my data already lives — is a natural next step."

## Application Notes
- Remote USA/Canada is a major advantage. No relocation required.
- Comp range ($154K-$185K) is the lowest of the three targets. Research Grafana Labs total comp (equity, benefits) and pre-IPO trajectory.
- Grafana Labs is an open-source company — open-source contributions are highly valued. Consider contributing to Grafana's AI/ML observability features before or during the application process.
- The "we already use your stack" angle is the strongest differentiator. No other candidate will walk in with production Grafana + OTel instrumentation for AI retrieval systems.
- Go is Grafana's primary backend language. If this role requires Go, flag the learning curve but emphasize Python/TypeScript as the languages of the GenAI ecosystem this role serves. Learning Rust already demonstrates systems-language appetite.
- This role may overlap with Grafana's "AI Observability" product direction (Grafana Cloud traces for LLM applications). Research their recent announcements in this space.
