# Proof Points — Master Evidence List

Citable numbers and evidence, organized by domain. Use these in application briefs, cover letters, and interview prep.

## Retrieval Quality

- **+22% precision** vs vanilla cosine similarity (P@5, controlled 20-concept domain)
- **+26% recall** vs vanilla cosine similarity (R@5)
- **+14% nDCG** vs vanilla cosine similarity (nDCG@5)
- Controlled benchmark: 20-concept authorization domain with distractors
- Three-signal composition: vector similarity + PPR + Thompson Sampling

## Performance / Overhead

- Graph explore overhead: **0.02ms median** (embedding is 99.5% of cost)
- Feedback recording: **<0.01ms median**
- MCP stdio transport: **29 tool calls in 3.94s** with ~400ms server spawn
- Graph explore adds negligible latency to retrieval pipeline

## Framework Adapters

- **7 framework adapters**: CrewAI, LangChain (Python), LangChain.js, Agno, AutoGen, Mastra, Smolagents
- All passing **framework-authored test suites** (not ours)
- **100+ CI tests** across all adapters
- Zero-skip CI policy: latest-version framework pinning
- One import swap — implements the framework's native interface

## Thompson Sampling / Learning

- Beta-Bernoulli posteriors on knowledge graph edge weights
- **30 days of production posterior divergence** as evidence
- Convergence analysis published with interactive Beta distribution widget
- Same math as TensorZero's contextual bandits (different application layer)

## Infrastructure / Containment

- **bilrost**: single-command VM provisioning (`bilrost up/status/ssh`)
- Profile-based management, PyPI distribution
- STRIDE threat model for agent containment
- Lima VM with network isolation
- Published on PyPI: qortex, bilrost

## Observability

- Full OTel instrumentation: Grafana, Jaeger, Prometheus
- Traces span from query → retrieval → graph explore → feedback
- qortex-observe: OpenTelemetry integration for agent behavior monitoring
- Dashboard: retrieval latency, feedback rates, posterior drift

## Publications / Content

- Feedback loop article with convergence analysis and 3 hand-drawn SVG diagrams
- Interactive Beta distribution widget (Canvas API, Lanczos lnGamma approximation)
- Annotation components for technical writing
- Article byline: "Peleke (ed: Claude)" — human-AI collaborative writing

## Prior Experience

- **edX**: Education platform, content delivery at scale
- **Linguistics/Philology**: Classical Latin, Old Norse/Icelandic, morphosyntactic analysis
- **Interlinear**: Adaptive language learning app, CLTK + Reynir NLP pipelines
- **Swae OS**: Federated GraphQL mesh across health data sources

## Open Source

- Published on PyPI: qortex, bilrost
- mkdocs documentation sites on GitHub Pages
- CI: latest-version framework pinning, zero-skip policy
- Framework adapters as distribution strategy
