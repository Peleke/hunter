# ML / AI Engineer Profile

## One-liner
ML engineer who built a knowledge graph with adaptive retrieval, framework adapters for 7 agent platforms, and a feedback loop that makes retrieval learn from usage.

## Pitch (2-3 sentences)
I build retrieval systems that improve from feedback. qortex composes vector similarity, Personalized PageRank over typed edges, and Thompson Sampling into a retrieval layer that updates itself from usage. It drops into CrewAI, LangChain, Agno, AutoGen, and Mastra via native interface adapters — passing their own test suites, not mine.

## Resume emphasis order
1. qortex — knowledge graph, PPR, Thompson Sampling, Beta-Bernoulli posteriors
2. Retrieval quality — +22% precision, +26% recall, +14% nDCG vs vanilla cosine
3. Framework adapters — 7 frameworks, native interfaces, 100+ CI tests
4. MCP transport — cross-language via stdio, TypeScript adapters (Mastra, LangChain.js)
5. Embeddings + vector search — the baseline that graph retrieval builds on
6. NLP background — CLTK, Reynir, morphosyntactic error classification (Interlinear)

## Proof points
- Controlled retrieval benchmark: 20-concept auth domain, qortex vs vanilla cosine
- Graph explore overhead: 0.02ms median (embedding is 99.5% of cost)
- 7 framework adapters, all passing framework-authored test suites
- MCP stdio transport: 29 tool calls in 3.94s with ~400ms server spawn
- Feedback recording: <0.01ms median

## "If they ask about X, talk about Y"
- **"How does your retrieval work?"** → Three signals: vector similarity (baseline), PPR over typed edges (structural), Thompson Sampling on edge weights (adaptive). They compose.
- **"What's novel?"** → Applying Thompson Sampling to retrieval edge weights. The math is well-studied for bandits. Using it to make a knowledge graph learn which paths matter from usage feedback is new.
- **"How do you evaluate?"** → Precision@k, Recall@k, nDCG@k on a controlled domain with distractors. Overhead benchmarks. Framework test suite conformance. Not vibes.
- **"NLP experience?"** → Interlinear: adaptive language learning app. CLTK + Reynir NLP pipelines for Classical Latin and Icelandic. Morphosyntactic error classification feeding Thompson Sampling for concept mastery.

## Target companies
Anthropic, Cohere, Pinecone, Weaviate, Weights & Biases, Hugging Face, LangChain, Databricks
