# Grafana Labs — Senior SE, GenAI & ML Evaluation Frameworks
## READY-TO-SUBMIT APPLICATION PACKAGE

**Posting**: https://job-boards.greenhouse.io/grafanalabs/jobs/5559973004
**Status**: READY TO SUBMIT
**Comp range**: $148,505 - $178,206 (base) + RSUs + benefits
**Location**: Remote, USA

---

## Cover Letter

Dear Grafana Labs Hiring Team,

I run Grafana, Jaeger, and Prometheus in production to observe AI retrieval systems. qortex-observe instruments the full pipeline with OpenTelemetry -- traces from query through graph explore to feedback, dashboards tracking posterior drift over 30 days. When I saw this role, I recognized the job description as a summary of what I've already built on your stack.

My retrieval system composes vector similarity, Personalized PageRank, and Thompson Sampling into a three-signal retrieval layer. I evaluate it with P@5, R@5, and nDCG@5 on a controlled 20-concept domain with distractors -- not just positive examples, because without distractors every retrieval system looks good. Results: +22% precision, +26% recall, +14% nDCG versus vanilla cosine similarity. These numbers come from golden test sets with known ground truth, the same evaluation methodology this role asks you to build as a platform capability.

On the CI/CD side, I maintain 100+ tests across 7 framework adapters (CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, LangChain.js) with a zero-skip policy and latest-version framework pinning. If CrewAI ships a breaking change on Tuesday, CI fails on Tuesday. This is evaluation-in-CI at the speed frameworks ship -- exactly the regression tracking pipeline this role requires.

For structured output evaluation, I use Thompson Sampling with Beta-Bernoulli posteriors on knowledge graph edge weights, tracking convergence over 30 days of production data. This is a principled complement to LLM-as-judge methods: statistical evaluation where you can get closed-form answers, LLM-as-judge where you need open-ended assessment, and inter-rater reliability metrics to calibrate both.

Grafana Labs built the observability platform I chose independently. This role is the intersection of AI evaluation expertise and the Grafana stack -- and I'm already standing at that intersection. I'd welcome the opportunity to build GenAI evaluation frameworks on the platform where my data already lives.

Peleke Sengstacke

---

## Application Form Notes

Greenhouse standard application fields for this posting:

| Field | What to Enter |
|-------|---------------|
| **First Name** | Peleke |
| **Last Name** | Sengstacke |
| **Email** | (personal email) |
| **Phone** | (personal phone) |
| **Resume** | Upload current resume PDF |
| **Cover Letter** | Paste the cover letter above (or upload as PDF) |
| **LinkedIn** | LinkedIn profile URL (ensure profile is current and public) |
| **Website / Portfolio** | https://peleke.dev |
| **GitHub** | https://github.com/Peleke |
| **How did you hear about us?** | "Job board / Greenhouse careers page. I also use Grafana, Jaeger, and Prometheus in my own AI observability stack (qortex-observe), so I actively follow Grafana Labs' product direction." |
| **Location** | Syracuse, NY (remote -- willing to travel for team offsites) |
| **Are you authorized to work in the US?** | Yes |
| **Do you now or will you require sponsorship?** | No |
| **Anything else?** | "I built qortex-observe on the Grafana/Jaeger/Prometheus stack for AI retrieval observability. Portfolio with technical writeups: https://peleke.dev" |

**Before submitting, verify:**
- [ ] Resume is updated with qortex-observe, retrieval benchmarks, and framework adapter CI details
- [ ] LinkedIn profile is public and matches resume
- [ ] Portfolio site (peleke.dev) is live and loading correctly
- [ ] Cover letter is pasted (not just uploaded) if Greenhouse provides a text field

---

## Key Links to Include

| Link | URL | Where to Use |
|------|-----|-------------|
| Portfolio | https://peleke.dev | Website/Portfolio field, cover letter |
| GitHub | https://github.com/Peleke | GitHub field |
| LinkedIn | (your LinkedIn URL) | LinkedIn field |
| Greenhouse Posting | https://job-boards.greenhouse.io/grafanalabs/jobs/5559973004 | Reference only |

**Note on qortex repo visibility**: If the qortex repo or qortex-observe repo is public, include the direct GitHub link in the "Anything else" field. If private, the portfolio writeups at peleke.dev serve as the public proof layer.

**Note on published articles**: If the feedback loop / convergence analysis article is live on peleke.dev, include the direct URL. This is the single strongest artifact for this role -- it demonstrates golden test set design, regression tracking, and the Grafana observability angle all in one piece.

---

## Interview Prep Quick Sheet

### The 30-Second Pitch (for "Tell me about yourself")

> I'm a software engineer focused on AI retrieval evaluation and observability. I built qortex, a knowledge graph retrieval system that composes vector similarity, Personalized PageRank, and Thompson Sampling -- and I evaluate it with P@k and nDCG on controlled domains, not vibes. The observability layer runs on Grafana, Jaeger, and Prometheus with full OTel instrumentation. I maintain 100+ CI tests across 7 framework adapters with a zero-skip policy. This role asks me to build GenAI evaluation frameworks on the observability platform I already use daily -- that's why I'm here.

### 5 Most Likely Questions

**1. "Describe your experience with Grafana and observability."**

qortex-observe is a full OTel instrumentation layer for AI retrieval. Grafana for dashboards -- retrieval latency, feedback rates, posterior drift. Jaeger for distributed traces -- query to retrieval to graph explore to feedback. Prometheus for metrics collection. This is how I monitor whether Thompson Sampling actually improves retrieval quality over time. I chose the Grafana stack because it's the best open-source observability platform. Now I want to help build it.

**2. "How would you design an evaluation framework for GenAI systems?"**

Start with the evaluation taxonomy. Retrieval quality (P@k, nDCG) is different from response quality (LLM-as-judge) is different from system performance (latency, throughput). Each needs its own golden test set design, regression thresholds, and CI integration strategy. The pipeline: define golden set with distractors, instrument the system with OTel, run evaluation suite in CI, push metrics to Prometheus, visualize in Grafana, alert on regressions. The framework should make that pipeline a one-import setup for internal teams.

**3. "How do you approach golden test set design?"**

Controlled domains with known ground truth. My retrieval benchmark uses a 20-concept authorization domain with distractors -- concepts that are semantically similar but not relevant. I measure P@5, R@5, and nDCG@5, which catches both precision failures (wrong results) and ranking failures (right results in wrong order). Key insight: golden sets need distractors, not just positive examples. Without distractors, every retrieval system looks good.

**4. "What's your experience with CI/CD integration of evaluation?"**

100+ CI tests across 7 framework adapters. Latest-version framework pinning -- not locked versions, latest. Zero-skip policy: if a test is flaky, fix it, don't skip it. If CrewAI ships a breaking change on Tuesday, CI fails on Tuesday. For GenAI evaluation in CI, same philosophy: run evaluations on every commit, fail the build if quality regresses below thresholds.

**5. "LLM-as-judge -- what's your take?"**

It solves a real problem: evaluating open-ended generation where P@k doesn't apply. But it has calibration issues -- different models judge differently, same model judges differently across prompts. I complement LLM-as-judge with statistical methods: inter-rater reliability metrics, confidence intervals, and closed-form evaluation where possible (P@k, nDCG). Thompson Sampling is relevant here -- you can model judge reliability as a bandit problem and weight scores by posterior confidence.

### One Question to Ask Them

> "The job description mentions golden test sets and regression tracking. How does the team currently manage dataset versioning and golden set evolution -- do evaluation datasets live alongside code in version control, or is there a separate data management layer? I'm curious because in my experience, test set drift is the silent killer of evaluation pipelines."

This shows you've thought about the operational reality of evaluation at scale, not just the algorithms.

### Bonus Questions to Have Ready

- "What does the GenAI evaluation framework serve internally -- is it primarily for Grafana's own AI features (like AI-assisted dashboarding), or is it also a product offering for Grafana Cloud customers?"
- "How does the team think about the relationship between trace-level observability and batch evaluation? In my experience they inform each other -- traces surface failure modes that become golden test cases."

---

## Location Note

Currently in Syracuse, NY (temporary). Home base is Atlanta, GA. This is a remote role (USA/Canada) so location is not a concern. Willing to travel for team events, onboarding, and offsites as needed.

---

## Submission Checklist

- [ ] Resume PDF uploaded
- [ ] Cover letter pasted/uploaded
- [ ] LinkedIn URL entered (profile set to public)
- [ ] Portfolio URL entered: https://peleke.dev
- [ ] GitHub URL entered: https://github.com/Peleke
- [ ] "How did you hear about us" filled in
- [ ] All required fields completed
- [ ] Submit at https://job-boards.greenhouse.io/grafanalabs/jobs/5559973004
