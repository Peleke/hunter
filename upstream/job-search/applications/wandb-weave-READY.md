# Weights & Biases — Software Engineer, Weave | READY TO SUBMIT

**Status**: Ready to submit
**Date prepared**: 2026-02-23
**Posting**: https://jobs.lever.co/wandb/399888f7-31ee-4746-a351-bc135dac9344
**Location**: San Francisco, CA (Hybrid)
**Comp**: $177,000 - $245,000 base
**Company note**: W&B was acquired by CoreWeave (completed May 2025). Now part of CoreWeave's end-to-end AI platform — compute + developer tooling.

---

## Cover Letter

Dear Hiring Team,

I build the kind of agent systems Weave is designed to track — and I instrument them with evaluation pipelines. I'd bring a builder's perspective to Weave's developer experience.

qortex, the knowledge-graph retrieval system I built and maintain, ships adapters for seven agent frameworks — CrewAI, LangChain, LangChain.js, Agno, AutoGen, Mastra, and Smolagents — each implementing the framework's native interface so developers swap one import and gain graph-augmented retrieval with a feedback loop. I evaluate retrieval quality with controlled benchmarks: Precision@5, Recall@5, and nDCG@5 on a 20-concept domain with distractors. The results — +22% precision, +26% recall over vanilla cosine similarity — come from a three-signal composition of vector similarity, Personalized PageRank, and Thompson Sampling on Beta-Bernoulli posteriors. I track convergence over 30 days to verify the learning signal is real. This is the evaluation methodology Weave helps teams implement, and I've built it from the ground up.

The Python and TypeScript depth is not surface-level. Python is my primary language: the qortex core engine, the MCP server (29 tool calls in 3.94s over real stdio transport), bilrost (agent sandbox CLI), and all packages published to PyPI. TypeScript is production, not hobby: Mastra and LangChain.js adapters implement framework-native interfaces and pass their test suites. This cross-language fluency maps directly to Weave's need for SDK parity across ecosystems.

I also built qortex-observe — full OpenTelemetry instrumentation with Grafana, Jaeger, and Prometheus, tracing from query through retrieval to graph explore to feedback. That's a bespoke version of what Weave provides as a platform. I'd rather build the platform.

On the open-source side, my practice mirrors W&B's: PyPI-published packages, mkdocs documentation sites, CI with latest-version framework pinning and a zero-skip policy. If CrewAI ships a breaking change, my pipeline catches it the same day. Framework adapters are my distribution strategy — the same integration-first model Weave uses to meet developers where they work.

I'm excited about the CoreWeave acquisition strengthening the infrastructure underneath Weave. Compute plus developer tooling is the right combination for what AI teams actually need. I want to contribute to Weave's evaluation primitives and integrations from the inside, with the perspective of someone who builds and evaluates agent retrieval systems daily.

Peleke Sengstacke

---

## Application Form Notes

**Platform**: Lever (standard fields)

| Field | Value |
|-------|-------|
| Full Name | Peleke Sengstacke |
| Email | *(use primary email)* |
| Phone | *(use primary phone)* |
| Resume/CV | Upload current resume PDF |
| Portfolio / Website | https://peleke.dev |
| LinkedIn | *(use LinkedIn URL)* |
| GitHub | https://github.com/Peleke |
| Cover Letter | Paste cover letter above or upload |
| How did you hear about this role? | Lever job board / W&B careers page |
| Are you authorized to work in the US? | Yes |
| Do you now or will you in the future require sponsorship? | No |
| Location | Syracuse, NY (willing to relocate to San Francisco) |

**Note on Lever**: Lever forms sometimes include optional fields for "additional information" or "anything else you'd like us to know." If present, use the location note below.

---

## Key Links to Include

| Link | Context |
|------|---------|
| https://peleke.dev | Portfolio — case studies, technical writing, project overviews |
| https://github.com/Peleke | GitHub — open-source work, adapter implementations |
| https://pypi.org/project/qortex/ | PyPI — published qortex package |
| https://pypi.org/project/bilrost/ | PyPI — published bilrost agent sandbox CLI |
| https://peleke.dev/writing | Technical articles — feedback loops, convergence analysis, evaluation methodology |

---

## Interview Prep Quick Sheet

### 5 Most Likely Questions and Answers

**1. "Tell us about your experience building GenAI-powered applications."**

qortex is a knowledge-graph retrieval system that serves as the retrieval layer for GenAI agents. It composes three signals — vector similarity, Personalized PageRank, and Thompson Sampling with Beta-Bernoulli posteriors — to rank knowledge graph edges. I built native adapters for seven agent frameworks so any agent can use qortex as its retrieval backend with a single import swap. On the evaluation side, I run controlled benchmarks with P@5, R@5, and nDCG@5 on a 20-concept domain with distractors, and track posterior convergence over 30 days to verify the learning signal. The MCP server exposes the full system over stdio transport for tool-use workflows. This is end-to-end GenAI infrastructure: retrieval, evaluation, observability, and agent integration.

**2. "How do you think about building integrations with LLM frameworks like LangChain and LlamaIndex?"**

The key principle is: implement the interface the framework already defines. Don't make developers learn a new API. For LangChain, I implement BaseTool. For CrewAI, I implement their Tool class. For Mastra, I implement their TypeScript tool interface. Each adapter passes the framework's own test suite, not mine — that's the quality bar. I have 100+ CI tests across all seven adapters, with latest-version framework pinning and a zero-skip policy so breaking changes surface immediately. This is exactly the integration model Weave uses: meet developers where they already work, then add value transparently. I'd apply this same approach to Weave integrations with LlamaIndex, Instructor, and whatever frameworks emerge next.

**3. "Describe your Python and TypeScript experience. How deep does it go?"**

Python is my primary language. The qortex core — knowledge graph construction, PPR traversal, Thompson Sampling, Beta-Bernoulli posterior updates — is all Python. The MCP server, bilrost CLI, and evaluation benchmarks are Python. Both qortex and bilrost are published on PyPI with proper packaging, versioning, and CI. TypeScript is production-grade, not supplementary: the Mastra adapter and LangChain.js adapter implement TypeScript-native framework interfaces, handle async patterns, and pass framework test suites. The MCP server's stdio transport bridges both ecosystems. I ship production code in both languages, which is what Weave needs for cross-language SDK development and parity between the Python and TypeScript SDKs.

**4. "How would you approach building evaluation features for Weave?"**

From the builder's perspective. Most evaluation platforms treat RAG as a black box, but retrieval quality, context relevance, and response quality are separate signals that need separate measurement. I'd push for first-class retrieval evaluation in Weave: P@k and nDCG tracking over time, context window utilization metrics, and feedback loop instrumentation that shows whether systems are actually learning. I'd also ensure the Evaluation Playground supports custom scorer functions that go beyond LLM-as-judge — sometimes you need deterministic metrics like exact-match recall on known ground truth. And TypeScript SDK parity is non-negotiable; too many AI tools are Python-first, TypeScript-eventually.

**5. "Why W&B? Why now?"**

W&B has the credibility and distribution to make GenAI evaluation a standard practice, not an afterthought. Weave's positioning — open-source, framework-agnostic, traces plus evaluation — is exactly right. The CoreWeave acquisition adds compute infrastructure underneath the developer tooling, which means Weave can offer end-to-end value from training to evaluation to production monitoring. I want to build evaluation tools from the perspective of someone who builds the systems being evaluated. That dual perspective — builder and instrumenter — is rare on platform teams, and it's what makes developer tools actually useful.

### 30-Second Pitch

"I build agent retrieval systems and evaluate them — Thompson Sampling on knowledge graphs, controlled benchmarks with P@5 and nDCG, OTel observability from query to feedback. I ship adapters for seven agent frameworks in Python and TypeScript, all passing framework test suites, all published on PyPI with CI. Weave tracks exactly the kind of systems I build. I want to bring that builder's perspective to the platform side — making Weave's evaluation primitives, integrations, and SDK better because I've felt the pain from the other direction."

### One Question to Ask Them

"Weave's integration model — meeting developers inside their existing framework — is core to adoption. How does the team decide which frameworks and libraries to prioritize for new integrations, and how do you balance breadth of integration coverage against depth of support for each one?"

*This question signals understanding of their distribution strategy, shows you think about prioritization trade-offs, and opens a conversation where your seven-adapter experience becomes directly relevant.*

---

## Location Note

Currently in Syracuse, NY (temporary); home base is Atlanta, GA. Open to relocating to San Francisco for a hybrid role. Realistic relocation timeline: 2-4 weeks from offer acceptance. Happy to discuss schedule and logistics.

**Re: CoreWeave acquisition** — Aware that Weights & Biases was acquired by CoreWeave (completed May 2025). Excited about the combined platform: CoreWeave's compute infrastructure underneath W&B's developer tooling creates a genuinely end-to-end AI development platform. This strengthens the case for Weave as the evaluation and observability layer — it's no longer just a standalone tool, it's part of the stack.
