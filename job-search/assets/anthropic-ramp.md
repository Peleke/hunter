# Anthropic Interview Prep — 6-9 Month Ramp

Target roles: AI Observability RE ($320-405K), Platform/Toolbox SWE ($320-405K), Autonomous Agent Infra ($320-485K)

## The Honest Assessment

**What you have (most candidates don't):**
- Novel applied research: Thompson Sampling on retrieval edges — nobody else is doing this
- Published technical writing with real data (convergence plots, interactive widgets, not vibes)
- Production OTel instrumentation on AI systems (AI Obs team's exact problem)
- MCP implementation experience (Toolbox team's exact protocol)
- 7 framework adapters passing framework-authored test suites (engineering discipline signal)
- Feedback loop article, hand-drawn SVG diagrams, interactive Beta distribution widget

**What you're missing:**
- DSA fluency under time pressure
- ML theory crispness (you USE the concepts but can't whiteboard proofs on demand)
- System design interview muscle (you can design systems, you haven't practiced the 45-minute format)
- Scale stories ("here's how I'd take qortex to 10M queries/day")
- Coding speed (clean Python solutions to hard problems in 30 min)

## Phase 1: Foundation (Months 1-2)

### DSA — Build the Floor

**Resource**: Neetcode 150 (neetcode.io)
**Pace**: 2-3 problems/day, Python only
**Focus order**:
1. Arrays & Hashing (weeks 1-2) — this is the foundation for everything
2. Two Pointers + Sliding Window (weeks 2-3) — pattern recognition
3. Stack (week 3) — clean thinking
4. Binary Search (week 4) — precision
5. Trees (weeks 5-6) — recursive thinking
6. Graphs (weeks 7-8) — you already think in graphs (qortex)

**Method**:
- Set a 25-minute timer. If you can't solve it, read the solution.
- Understand WHY the solution works. Write the intuition in one sentence.
- Re-solve from scratch the next day without looking.
- Track: date, problem, solved (Y/N), time, pattern category

**Target by end of Month 2**: Solve any Neetcode medium in 20 minutes cold.

### ML Fundamentals — Get Crisp

**Resource**: Chip Huyen "Machine Learning Interviews" book + Andrew Ng's CS229 notes
**Pace**: 30 min/day reading + notes
**Topics in order**:
1. Supervised learning basics (bias-variance, overfitting, regularization)
2. Evaluation metrics (precision, recall, F1, nDCG — you know these, now explain them cleanly)
3. Gradient descent + optimization (SGD, Adam, learning rate schedules)
4. Probabilistic models (Bayesian inference, conjugate priors — THIS IS YOUR STRENGTH, sharpen it)
5. Bandits & reinforcement learning (Thompson Sampling, UCB, regret bounds — own this chapter)
6. Information retrieval (TF-IDF, BM25, embedding models, re-ranking — own this too)

**Method**:
- For each topic, write a 1-paragraph explanation as if it's a section of your feedback loop article
- Practice explaining it out loud (record yourself, cringe, get better)
- Know the math well enough to write it on a whiteboard, not well enough to prove theorems

**Target by end of Month 2**: Explain any ML fundamental in 2 minutes without notes.

## Phase 2: Interview Patterns (Months 3-4)

### DSA — Push to Hard

**Resource**: Neetcode 150 (finish it) + blind 75 hard problems
**Pace**: 2 problems/day, one medium one hard
**Focus**:
1. Dynamic Programming (weeks 1-3) — the big boss. Start with 1D, then 2D, then optimization.
2. Heap / Priority Queue (week 4) — scheduling problems
3. Advanced Graphs (weeks 5-6) — Dijkstra, topological sort, union find
4. Intervals + Greedy (week 7) — pattern matching
5. Tries + Backtracking (week 8) — completeness

**Method**:
- Hard problems: 45 minutes max before reading solution
- Focus on recognizing the PATTERN, not memorizing solutions
- After solving: write a one-line note on which pattern this uses
- Weekly review: re-solve 5 problems from previous weeks

**Target by end of Month 4**: Solve any Neetcode hard in 35 minutes. Not all of them — but most.

### System Design — Build the Muscle

**Resource**: "Designing Data-Intensive Applications" (Kleppmann) + Alex Xu "System Design Interview"
**Pace**: 1 design session per week (timed, 45 minutes)
**Method**:
1. Pick a system (start with qortex — you already know it)
2. Set 45-minute timer
3. Talk out loud. Requirements → high-level design → deep dive → tradeoffs
4. Record yourself. Listen back. Notice where you ramble or get vague.
5. Practice with a friend if possible (Pramp, interviewing.io, or just a buddy)

**Systems to practice** (in order):
1. qortex at scale (retrieval system with feedback loop, 10M queries/day)
2. Agent observability platform (Anthropic AI Obs — literally the job)
3. MCP gateway (multi-tenant, isolated execution environments — Toolbox team)
4. Distributed knowledge graph with real-time updates
5. ML model serving platform with A/B testing and bandit routing
6. Event-driven agent monitoring system (traces, alerts, anomaly detection)

**Target by end of Month 4**: Deliver a clean 45-minute system design for any of these without notes.

## Phase 3: Anthropic-Specific Prep (Months 5-6)

### Research the Teams

- Read every Anthropic engineering blog post (https://www.anthropic.com/research)
- Understand Claude's architecture at a high level (Constitutional AI, RLHF, context windows)
- Read the MCP specification (you already know it, but be able to discuss design decisions)
- Read Anthropic's safety research (not to become an expert, but to speak fluently about alignment)

### Practice the Anthropic Interview Format

Anthropic interviews typically include:
1. **Phone screen** — 30 min, background + motivation + high-level technical
2. **Technical screen** — 45-60 min, coding problem (medium-hard Python)
3. **System design** — 45-60 min, design a system relevant to the team
4. **ML/Research deep-dive** — discussion of your work, go deep on one project
5. **Team/culture fit** — collaboration, values, why Anthropic

**For each round, practice**:
- Phone screen: your 2-minute pitch, tailored to the specific role
- Technical: 3 practice problems per week, timed, in a Google Doc (not an IDE)
- System design: the 6 systems above, one per week
- Deep-dive: practice walking through Thompson Sampling + PPR + convergence in 15 minutes
- Culture: why safety matters to you (genuinely — not rehearsed corporate answers)

### Polish Your Portfolio

By month 5-6, you should have:
- The feedback loop article (done)
- The "Agents Don't Learn" polemic (in progress)
- 2-3 more technical articles (from the 5-part series?)
- qortex docs polished and deployed
- Lab page with screenshots (Grafana, Jaeger, Memgraph dashboards)
- A clean GitHub profile with pinned repos

### Apply

**Month 6**: Apply to AI Observability and/or Platform/Toolbox specifically.
- You have a Tier A job (income, "currently employed" signal)
- Your portfolio has 6 months more published work
- Your DSA is at hard level
- Your system design is practiced
- Your ML fundamentals are crisp

**Month 7-9**: Interview. You're ready.

## The Performance Ramp: Camera → Twitch → Interviews

The interview is a performance. Train it like one.

### Stage 1: Private Camera (Months 1-2)
- Solve problems on camera (phone recording, screen share, whatever)
- Talk out loud. Explain your thinking. Get comfortable being watched.
- Weekly "test": pick a random medium, 25-minute timer, film it start to finish
- Watch it back. Notice where you go silent, where you panic, where you ramble
- Target: solve a medium on camera without freezing

### Stage 2: Recorded Reviews (Months 3-4)
- Record yourself doing hard problems. Edit if you want, or post raw.
- Share with friends for feedback (or a small Discord)
- Weekly test: 1 hard problem, 45-minute timer, filmed
- Target: clean narration through a hard problem, even if you don't solve it

### Stage 3: Live on Twitch (Months 5+)
- Stream DSA sessions live. Chat can watch you think.
- This is the final boss of interview anxiety — if you can solve problems with randos roasting you, a quiet Zoom with a recruiter is a vacation
- Bonus: this builds an audience and a reputation ("the guy who preps in public")
- Weekly: 1-2 live sessions, mix of medium and hard
- Target: confident enough to say "I stream this live, here's my Twitch" in an interview

### Why this works
Interview anxiety = performing under observation. Every stage increases the observation pressure until the actual interview feels easy by comparison. By month 6 you're not "prepping for interviews," you're a guy who solves problems on camera for fun.

## Weekly Schedule Template

| Day | Morning (1hr) | Evening (30min) |
|-----|--------------|----------------|
| Mon | DSA: 2 problems | ML reading + notes |
| Tue | DSA: 2 problems | ML reading + notes |
| Wed | System design mock (45 min) | Review DSA from Monday |
| Thu | DSA: 2 problems | ML reading + notes |
| Fri | DSA: 1 hard problem (45 min) | Re-solve week's hardest problem |
| Sat | System design: practice walk-through | Anthropic blog post reading |
| Sun | Rest or light review | Rest |

Total: ~10 hours/week. Sustainable alongside a job.

## Resources

- **DSA**: neetcode.io, LeetCode premium (optional), "Cracking the Coding Interview" (reference)
- **ML**: Chip Huyen "ML Interviews", CS229 notes, Sutton & Barto ch. 2-3 (bandits), Bishop ch. 1-4 (fundamentals)
- **System Design**: Kleppmann "DDIA", Alex Xu "System Design Interview" vol 1 & 2, ByteByteGo YouTube
- **Mock Interviews**: Pramp (free), interviewing.io, or trade with a friend
- **Anthropic-Specific**: anthropic.com/research blog, MCP spec, Claude model card

## Success Metrics

| Month | DSA | ML | System Design | Performance | Portfolio |
|-------|-----|-----|--------------|-------------|-----------|
| 1 | Can solve easies cold | Reading fundamentals | — | Solving on camera (private) | — |
| 2 | Mediums in 20 min | Explain any fundamental in 2 min | First mock (qortex) | Weekly filmed tests | — |
| 3 | Mediums in 15 min, starting hards | Whiteboard basics | 2 more mocks | Recorded reviews | 1 new article |
| 4 | Hards in 35 min | Crisp on bandits + IR | Clean 45-min session | Sharing recordings | 2 articles |
| 5 | Consistent hard solves | Own the Thompson Sampling chapter | 4 practiced systems | **Twitch live sessions** | Lab screenshots done |
| 6 | Interview-ready | Interview-ready | 6 systems done | Twitch regular | 3+ articles, clean GH |

## The Mindset

You are not starting from zero. You built a novel retrieval system. You published real data. You instrumented it with OTel. You wrote adapters for 7 frameworks. You just haven't practiced the interview format.

The interview is a performance. The work is real. Prepare for the performance.
