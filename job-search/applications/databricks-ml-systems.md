# Databricks — Senior Applied AI Engineer, ML for Systems & Infrastructure

## Role Summary
- **Link**: https://www.databricks.com/company/careers/engineering---pipeline/senior-applied-ai-engineer--ml-for-systems--infrastructure-7859597002
- **Location**: Mountain View, CA
- **Comp**: Not listed
- **Fit Score**: 5/5

## Lead Profile
**ML / AI Engineer.** This role explicitly asks for applying ML scheduling and optimization algorithms to improve engineering systems. That is a literal description of what qortex does: Thompson Sampling (an optimization algorithm from the bandit literature) applied to retrieval infrastructure to make it self-improving. The ML Engineer profile foregrounds the algorithm work and the measured retrieval quality gains.

## Tailored Pitch (2-3 sentences)
I apply optimization algorithms from the bandit literature to make retrieval infrastructure learn from usage. qortex composes Thompson Sampling with Personalized PageRank over typed knowledge graph edges, producing +22% precision and +26% recall vs vanilla cosine — with 0.02ms median graph explore overhead. This is applied ML research that ships as production infrastructure: 7 framework adapters, 100+ CI tests, PyPI distribution.

## Resume Emphasis
1. **qortex — Thompson Sampling as systems optimization.** The role says "applying ML scheduling and optimization algorithms to improve engineering systems." Thompson Sampling on edge weights IS an optimization algorithm applied to retrieval infrastructure. Beta-Bernoulli posteriors update from usage feedback — no retraining, no LLM calls.
2. **Retrieval quality benchmarks.** +22% precision, +26% recall, +14% nDCG vs vanilla cosine on a controlled 20-concept domain with distractors. This is the evidence that the optimization algorithm works.
3. **Production performance at scale.** Graph explore adds 0.02ms median overhead. Feedback recording at <0.01ms. Embedding is 99.5% of cost. The optimization layer is effectively free.
4. **Framework adapters as deployment strategy.** 7 framework adapters (CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, LangChain.js) — all passing framework-authored test suites. This is how you deploy ML infrastructure into diverse production environments.
5. **MCP transport — cross-language production bridge.** 29 tool calls in 3.94s over stdio. Python engine serving TypeScript ecosystems. Infrastructure that works across language boundaries.
6. **bilrost — production containment infrastructure.** Single-command VM provisioning with STRIDE threat modeling. Tier 0 environments need containment guarantees for agent systems.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| Applying ML optimization algorithms to improve engineering systems | Thompson Sampling on knowledge graph edge weights; Beta-Bernoulli posteriors that update from usage feedback — the algorithm optimizes retrieval paths in real time | Direct match |
| Strong coding / SWE fundamentals | 7 framework adapters with 100+ CI tests, PyPI-published packages (qortex, bilrost), zero-skip CI policy with latest-version pinning | Strong |
| Deploying and scaling models in production | MCP server (29 calls/3.94s), framework adapters passing production test suites, graph explore at 0.02ms median overhead | Strong |
| Monitoring models in production | qortex-observe: full OTel instrumentation (Grafana, Jaeger, Prometheus), traces from query through retrieval to feedback, posterior drift dashboards | Strong |
| Infrastructure challenges in Tier 0 environments | bilrost VM sandbox with STRIDE threat model, network isolation, profile-based containment for agent workloads | Moderate — containment focus, not Databricks-scale distributed systems |
| ML scheduling algorithms | Thompson Sampling IS a scheduling algorithm — it allocates exploration vs exploitation across retrieval paths based on posterior uncertainty. Same math as contextual bandits used in job scheduling | Direct match |
| Cross-functional collaboration | Framework adapters require implementing 7 different API contracts designed by 7 different teams. edX experience in education platform engineering | Moderate |

## Why This Role (authentic)
Databricks is where data infrastructure meets ML, and this role sits at the intersection I've been building toward: applying ML algorithms to make systems better, not just using systems to serve ML. The specific language — "ML scheduling and optimization algorithms to improve engineering systems" — describes what I built with qortex before I saw this posting. Thompson Sampling is a scheduling algorithm. Applying it to retrieval edge weights to make infrastructure self-improving is the same class of problem as optimizing Spark job scheduling or resource allocation. I want to do this work at Databricks scale, on infrastructure that millions of data teams depend on.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No distributed systems at Databricks scale (Spark, Delta Lake) | The optimization algorithm work transfers directly. Thompson Sampling on retrieval edges and Thompson Sampling on cluster scheduling are the same math at different scales. I learn infrastructure fast — went from zero to PyPI-published VM provisioning tool in weeks. |
| No explicit Spark / data engineering background | Frame as a strength: fresh perspective on applying modern ML optimization to data infrastructure. The role asks for ML engineers, not Spark engineers. |
| Single-developer projects, not large team experience | edX was a large engineering org. Framework adapters require conformance to 7 external teams' API contracts — that's cross-team collaboration by design. |
| Mountain View location (relocation may be needed) | Address directly if asked. Willing to relocate for the right role. Databricks' Mountain View campus is the right role. |

## If They Ask X, Say Y
1. **"How does Thompson Sampling apply to systems infrastructure?"** → Thompson Sampling solves the explore-exploit tradeoff. In retrieval, it decides which knowledge graph paths to prioritize based on posterior uncertainty. In systems infrastructure, the same algorithm decides which scheduling policies, resource allocations, or routing strategies to explore vs exploit. I've implemented it end-to-end: Beta-Bernoulli posteriors, feedback recording at <0.01ms, convergence analysis over 30 days.

2. **"What's your experience with production ML systems?"** → qortex runs a full production loop: query comes in, three-signal retrieval executes (vector + PPR + Thompson Sampling), results go to the agent, feedback updates posteriors. qortex-observe instruments this entire pipeline with OTel — Grafana dashboards track retrieval latency, feedback rates, and posterior drift. The MCP server handles 29 tool calls in 3.94s over real stdio transport.

3. **"Tell me about a time you improved system performance with ML."** → Vanilla cosine similarity was our baseline retrieval. I added two layers: Personalized PageRank over typed edges for structural context, then Thompson Sampling on edge weights for adaptive learning. Controlled benchmark on a 20-concept domain: +22% precision, +26% recall, +14% nDCG. The graph explore layer adds 0.02ms median — embedding remains 99.5% of cost. The ML optimization is effectively free in latency terms.

4. **"How do you evaluate your work?"** → Controlled benchmarks with distractors. Precision@k, Recall@k, nDCG@k on a domain where I know ground truth. Overhead benchmarks with median and percentile reporting. Convergence analysis on posterior divergence over time. Framework test suite conformance — not my tests, theirs. I measure before I claim.

5. **"What draws you to Databricks specifically?"** → The job description says "applying ML scheduling and optimization algorithms to improve engineering systems." I built that exact thing before I saw this posting. Thompson Sampling on retrieval edge weights is an optimization algorithm applied to engineering infrastructure. Doing this at Databricks scale — where the systems serve millions of data teams — is the version of this work that matters most.

6. **"How do you handle the transition from single-developer to large-team infrastructure?"** → My framework adapters are built to conform to external API contracts — 7 different frameworks, each with their own interface expectations and test suites. That's the same muscle as working within a large codebase with established conventions. I also have edX experience, which was a large engineering organization with production education infrastructure.

## Cover Letter Hooks
1. "The role description says 'applying ML scheduling and optimization algorithms to improve engineering systems.' I've been doing exactly that — Thompson Sampling on knowledge graph edge weights produces +22% precision over vanilla cosine, at 0.02ms median overhead. The algorithm is well-studied; applying it to make retrieval infrastructure self-improving is the work I want to bring to Databricks scale."

2. "Most retrieval systems are static — you embed, you search, you hope. I built one that learns. qortex applies Beta-Bernoulli posteriors to knowledge graph edges so the system discovers which retrieval paths matter from usage feedback, not retraining. This is applied ML research that ships as infrastructure, and Databricks is where infrastructure meets ML."

3. "I apply bandit algorithms to systems problems. Thompson Sampling decides which knowledge graph paths to explore vs exploit. The same optimization framework applies to cluster scheduling, resource allocation, and query routing — the core infrastructure challenges this role targets at Databricks."

## Application Notes
- Mountain View location — confirm relocation willingness early.
- Databricks is a Tier 1 target company listed in the ML Engineer profile.
- The "ML for Systems" framing is the strongest possible angle — lead every conversation with Thompson Sampling as optimization, not as "AI/ML."
- Databricks likely uses similar bandit/optimization approaches internally for Spark scheduling and resource management. Research their published papers on ML-driven infrastructure optimization before interviewing.
- Salary not listed — Databricks senior engineer comp is typically $250K-$400K+ total (base + equity). Research current levels on levels.fyi.
