# Research Engineer Profile

## One-liner
Research engineer building instrumented agent systems that learn from feedback, with early evidence of principled agentic learning over context.

## Pitch (2-3 sentences)
I build agent systems and study how they learn. My current work (qortex) implements Thompson Sampling over knowledge graph edges to create a retrieval layer that updates itself from usage — no retraining, no LLM calls. Agents instrumented with qortex learn which context components matter for which queries in real time, with 30 days of production posterior divergence as evidence.

## Resume emphasis order
1. qortex — adaptive learning layer, Thompson Sampling, Beta-Bernoulli posteriors, PPR
2. Evaluation + measurement — closed-loop measurement, convergence analysis, benchmark suite
3. Framework adapters — 7 frameworks, 100+ CI tests, interface conformance
4. Vindler — production loop, OTel instrumentation, real usage data
5. bilrost — containment, threat modeling, STRIDE analysis
6. Prior: edX, language background, philology

## Proof points
- Thompson Sampling on edge weights → measurable posterior divergence over 30 days
- Retrieval quality: +22% precision, +26% recall, +14% nDCG vs vanilla cosine
- 7 framework adapters passing their own test suites (not ours)
- Interactive Beta distribution widget in published article
- Feedback loop article with convergence analysis

## "If they ask about X, talk about Y"
- **"Tell me about your research"** → The learning gap in agentic AI. Agents don't learn. Here's the mechanism that makes them learn. Here's the data.
- **"What's your most technically challenging work?"** → Thompson Sampling + PPR composition for retrieval that updates from feedback. The math is well-studied; applying it to agent context engineering is new.
- **"How do you evaluate your work?"** → Closed-loop measurement. Precision/recall/nDCG on a controlled domain. Convergence plots. Before/after. Not vibes.
- **"Why this role?"** → I want to work on the systems that make AI systems better. The meta-layer. How do you make agents that improve from use?
- **"Biggest failure?"** → buildlog. 16K lines of Python trying to solve the wrong problem. What survived: the gauntlet (rule-based review). What I learned: the problem is bandit selection over context, not LLM-as-judge.

## Target companies
Anthropic, DeepMind, OpenAI (research), Cohere, AI2, FAIR, TensorZero
