# Weights & Biases — Software Engineer, Weave

## Role Summary
- **Link**: https://jobs.lever.co/wandb/399888f7-31ee-4746-a351-bc135dac9344
- **Location**: San Francisco, CA (hybrid)
- **Comp**: $177,000 - $245,000
- **Fit Score**: 5/5

## Lead Profile
**Developer Tools / DevEx**, supplemented by **ML Engineer.** Weave is an open-source toolkit for tracking and evaluating GenAI applications. The role calls for deep Python and TypeScript, building GenAI-powered applications, and multi-modal evaluations. The Dev Tools profile foregrounds the SDK/adapter/framework integration work. The ML profile provides the domain credibility — the candidate doesn't just build developer tools for AI, they build the AI systems those tools track.

## Tailored Pitch (2-3 sentences)
I build the kind of agent systems Weave is designed to track — and I instrument them end-to-end. qortex ships adapters for 7 agent frameworks in Python and TypeScript, each passing the framework's own test suite, with full OTel observability from query through retrieval to feedback. I bring deep Python and TypeScript, open-source distribution practice (PyPI, mkdocs, CI), and firsthand understanding of what developers building GenAI applications actually need to measure.

## Resume Emphasis
1. **Framework adapters — the Weave integration pattern.** 7 adapters (CrewAI, LangChain, LangChain.js, Agno, AutoGen, Mastra, Smolagents) implementing native interfaces. This is exactly Weave's integration model: meet developers where they already work. One import swap. All passing framework-authored test suites.
2. **Python + TypeScript depth.** Core engine in Python. Mastra and LangChain.js adapters in TypeScript. MCP server bridging both ecosystems over stdio. This is not "I've used both" — it's "I ship production code in both."
3. **Evaluation pipeline expertise.** Controlled retrieval benchmarks: P@5, R@5, nDCG@5 on a 20-concept domain with distractors. Convergence analysis on posterior divergence. This is the evaluation methodology Weave helps teams implement.
4. **Open-source practice.** PyPI-published packages (qortex, bilrost). mkdocs documentation sites. CI with latest-version framework pinning and zero-skip policy. This matches W&B's open-source culture and distribution model.
5. **MCP server — cross-language tooling.** 29 tool calls in 3.94s. Real stdio transport. Tool schema definition and validation. This is SDK/tooling infrastructure work.
6. **qortex-observe — GenAI observability.** Full OTel instrumentation: Grafana, Jaeger, Prometheus. Traces from query to retrieval to graph explore to feedback. The kind of telemetry Weave captures, built from scratch.

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| 4+ years SWE experience | edX (education platform engineering) + qortex ecosystem (knowledge graph, 7 adapters, MCP server, bilrost, observe) + Interlinear (NLP app) | Strong |
| Deep Python | qortex core: knowledge graph, PPR, Thompson Sampling, Beta-Bernoulli posteriors. bilrost CLI. MCP server. All Python. PyPI published. | Direct match |
| Deep TypeScript | Mastra adapter, LangChain.js adapter, MCP stdio transport for TypeScript ecosystems. Production TypeScript, not hobby projects. | Strong |
| Building GenAI-powered applications | qortex is the retrieval layer for GenAI agents. 7 framework adapters power agent RAG pipelines. MCP server provides tool access. | Direct match |
| Multi-modal evaluations | Retrieval evaluation: P@k, R@k, nDCG@k. Performance evaluation: latency percentiles. Convergence evaluation: posterior drift over 30 days. Three evaluation dimensions, quantified. | Strong |
| LLM playgrounds / dataset editing | Controlled 20-concept benchmark domain with distractors. Dataset construction for retrieval evaluation. Interactive Beta distribution widget for convergence visualization. | Moderate — evaluation datasets, not playground UI |
| Open-source toolkit sensibility | PyPI packages, mkdocs sites, CI with latest-version pinning, framework test suite conformance. Ships like open-source because it is. | Strong |

## Why This Role (authentic)
Weave tracks exactly the kind of systems I build. When I instrument qortex agents with OTel traces from query through retrieval to feedback, I'm building a bespoke version of what Weave provides as a platform. I've felt the pain of evaluating GenAI applications from the builder side — precision@k and nDCG are necessary but not sufficient; you need traces, you need feedback loops, you need to see how retrieval quality changes over time. Weave is building the tool I wish I had. Contributing to it from the inside, with the perspective of someone who builds and evaluates agent retrieval systems daily, is the role where my experience compounds fastest.

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| No direct experience building evaluation platform UIs (playgrounds, dataset editors) | I've built the evaluation methodology and the data that feeds these UIs. Understanding what developers need to see (P@k, traces, posterior drift) is more valuable than UI framework experience, which I can learn fast. Interactive Beta distribution widget shows frontend data visualization capability. |
| Single-developer projects vs team-scale SWE | Framework adapters require conforming to 7 external API contracts — that's cross-team interface design. edX was a large engineering org. Open-source packaging (PyPI, CI, docs) follows the same rigor as team-scale development. |
| San Francisco hybrid — relocation may apply | Address directly. San Francisco hybrid is workable. |
| W&B product-specific knowledge (Weave SDK internals) | I'm a user-perspective hire. I build the systems Weave tracks. Picking up the SDK internals is fast when you already understand the domain deeply. |

## If They Ask X, Say Y
1. **"How do you think about GenAI evaluation?"** → Three dimensions. Retrieval quality: P@k, R@k, nDCG@k on controlled domains with known ground truth. Performance: latency percentiles at each pipeline stage (embedding, graph explore, feedback recording). Learning: posterior convergence over time — are the Thompson Sampling weights actually diverging from priors? Each dimension needs its own measurement methodology. Weave can unify all three in one trace.

2. **"Walk me through your Python and TypeScript experience."** → Python is my primary language. qortex core: knowledge graph construction, PPR traversal, Thompson Sampling with Beta-Bernoulli posteriors, MCP server, bilrost CLI — all Python, all PyPI-published. TypeScript: Mastra adapter, LangChain.js adapter, MCP stdio transport layer. These aren't ports — they implement TypeScript-native framework interfaces. I ship production code in both ecosystems, which is exactly what Weave needs for cross-language SDK support.

3. **"What would you build for Weave?"** → Better evaluation primitives for retrieval-augmented systems. Most eval frameworks treat RAG as a black box. But retrieval quality, context relevance, and response quality are separate signals that need separate evaluation. I'd push for first-class retrieval evaluation in Weave: P@k tracking over time, context window utilization metrics, feedback loop instrumentation. And I'd make sure the TypeScript SDK has full parity — too many AI tools are Python-first, TypeScript-eventually.

4. **"How do you approach open-source development?"** → Ship with distribution in mind. PyPI packages, mkdocs documentation, CI that pins latest framework versions with zero-skip policy. If CrewAI ships a breaking change, my CI catches it the same day. That's the rigor open-source users expect. I also think about adoption through integration — framework adapters are my distribution strategy, which is the same pattern Weave uses.

5. **"Describe your experience with agent frameworks."** → I've implemented native adapters for 7: CrewAI, LangChain (Python), LangChain.js, Agno, AutoGen, Mastra, Smolagents. Each adapter implements the framework's own interface — BaseTool, Tool, custom_tool, whatever the framework expects. All pass the framework's own test suites, not mine. 100+ CI tests total. This gives me deep knowledge of how agent developers actually build, which is critical for building evaluation tools that serve them.

6. **"Why W&B over other companies?"** → W&B has the credibility and the distribution to make GenAI evaluation a standard practice, not an afterthought. Weave's positioning — open-source, framework-agnostic, focused on traces and evaluation — is exactly right. I want to build evaluation tools from the perspective of someone who builds the systems being evaluated. That perspective is rare on most platform teams.

## Cover Letter Hooks
1. "I build the kind of agent systems Weave is designed to track. qortex ships retrieval adapters for 7 agent frameworks in Python and TypeScript, each passing framework-authored test suites. When I instrument these systems with OTel traces from query to feedback, I'm building a bespoke version of what Weave provides as a platform. I'd rather build the platform."

2. "Most AI evaluation tools are built by platform engineers who study agent systems from the outside. I build agent retrieval systems — Thompson Sampling on knowledge graph edges, +22% precision over vanilla cosine — and evaluate them with P@k, nDCG, and convergence analysis. Bringing that builder's perspective to Weave's evaluation primitives is the contribution I want to make."

3. "Deep Python, production TypeScript, 7 framework adapters, PyPI packages, and a zero-skip CI policy. I ship open-source tooling the way W&B ships open-source tooling. The match is the methodology, not just the tech stack."

4. "Weave needs engineers who understand both the developer tools layer and the GenAI systems layer. I've built both: framework adapters and MCP servers on the tools side, Thompson Sampling and knowledge graph retrieval on the ML side. That dual perspective is what makes evaluation tooling actually useful."

## Application Notes
- Comp range is $177K-$245K. This is base salary; W&B equity should be factored into total comp evaluation.
- San Francisco hybrid — confirm schedule flexibility (how many days in-office).
- W&B is listed as a target company in both the ML Engineer and Dev Tools profiles. This is a high-priority application.
- Weave is relatively new in W&B's product lineup — research recent Weave releases, blog posts, and GitHub activity before applying. Understanding the current state of the SDK shows genuine interest.
- The Python + TypeScript combination is a differentiator. Many candidates have one or the other. Having production code in both, with framework adapter experience, is uncommon.
- Open-source contributions to Weave before or during the application process would be a strong signal. Review the Weave GitHub repo for good first issues.
