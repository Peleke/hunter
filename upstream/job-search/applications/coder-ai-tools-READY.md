# Coder — Software Engineer, AI Tools | READY TO SUBMIT

**Status**: Ready for submission
**Date prepared**: 2026-02-23
**Posting source**: HN Who's Hiring, February 2026
**Apply at**: https://coder.com/careers (exact posting URL not found via search — check the careers page directly or look for "Software Engineer, AI Tools" listing)

---

## Cover Letter

Dear Coder hiring team,

I built a hardened VM sandbox for autonomous coding agents because I needed to answer the same question Coder is answering at production scale: how do you let an agent execute arbitrary code without it escaping the sandbox, exfiltrating data, or corrupting the filesystem?

The result is bilrost — a Lima VM with OverlayFS containment, Docker dual-container isolation, per-tool network policies via UFW, and gated filesystem sync that scans for secrets before anything leaves the sandbox. One command to provision: `bilrost up`. Published on PyPI. The threat model came first (STRIDE analysis with 7 documents and 5 proof-of-concept exploits), then the containment layers, then the developer experience. That sequence — security model, isolation primitives, simple interface — is exactly how I'd approach building agent-safe CDEs at Coder.

The sandbox is one layer. The full stack I've built maps directly to what the AI Tools team needs:

- **Agent runtime**: Vindler (openclaw) — production agent lifecycle management, 90 merged PRs. Start, dispatch tools, observe, terminate.
- **Observability**: qortex-observe — full OpenTelemetry pipeline (Grafana, Jaeger, Prometheus) tracing every agent action from invocation through execution through feedback. You can't safely host what you can't observe.
- **Integration surfaces**: An MCP server bridging Python and TypeScript over stdio transport (29 tool calls in 3.94s), plus 7 framework adapters (CrewAI, LangChain, Agno, AutoGen, Mastra, Smolagents, LangChain.js) — each implementing the framework's native interface so users swap one import. Coder's AI Tools team faces the same integration challenge: make CDEs work for every agent runtime without forcing each one to build a Coder-specific integration.

I should be direct about Go: TypeScript and Python are my primary languages, and I'm actively picking up Rust. Go is on the learning path, and it's syntactically simpler than the Rust I'm writing now. The concurrency model — goroutines and channels — maps to the async agent patterns I've already built. I'd expect to be writing production Go within weeks. The harder ramp is Coder's architecture and codebase, not the language.

The open-source angle matters to me. Both qortex and bilrost are published on PyPI. I run CI against the latest upstream framework versions with a zero-skip policy — if a dependency ships a breaking change on Monday, our build fails on Monday. I write mkdocs documentation sites, not just READMEs. Working on a project with 100k+ GitHub stars means that engineering discipline has real consequences for real users. That's the accountability I want.

Peleke Sengstacke

---

## Application Form Notes

**Likely required fields:**
- Full name: Peleke Sengstacke
- Email: (use primary email)
- Location: Syracuse, NY (temporarily) / Atlanta, GA (home base) — fully remote, happy to travel for team events
- Resume/CV: Upload current resume (emphasize bilrost, framework adapters, MCP server, qortex-observe, Vindler)
- Portfolio/Website: https://peleke.dev
- GitHub: https://github.com/Peleke
- LinkedIn: (include if on profile)
- Cover letter: Paste the cover letter above
- How did you hear about this role: Hacker News Who's Hiring (February 2026)
- Visa/work authorization: US citizen / authorized to work in the US (confirm as applicable)
- Free-text / "Why Coder?": Lead with the bilrost/sandbox parallel. The containment problem is the same problem — Coder solves it at scale for thousands of developers and their coding agents simultaneously.

**If there is a "What interests you about this role?" field:**
Coder is defining how AI agents operate inside development environments, and that's the exact problem I've been solving at smaller scale. bilrost exists because autonomous agents need containment before they need capabilities. The open-source culture (100k+ stars) and the focus on agent-safe CDEs are the intersection of what I've built and what I want to build next.

---

## Key Links to Include

| Link | Context |
|------|---------|
| https://peleke.dev | Portfolio — case studies, writing, lab projects |
| https://github.com/Peleke | GitHub — open-source projects |
| https://pypi.org/project/qortex/ | qortex on PyPI — retrieval engine with framework adapters |
| https://pypi.org/project/bilrost/ | bilrost on PyPI — hardened VM sandbox for agent containment |

---

## Interview Prep Quick Sheet

### 5 Most Likely Questions and Answers

**1. "Walk us through how you'd design a sandboxed environment for an autonomous coding agent."**

Start with the threat model. I used STRIDE for bilrost: what can the agent exfiltrate (network), corrupt (filesystem), exhaust (resources), or escalate (permissions)? Then build layered containment for each. Lima VM provides the outer process boundary. OverlayFS prevents persistent writes unless explicitly synced. UFW gives each tool its own network policy — the agent can hit the LLM API but not arbitrary endpoints. Gated sync means code changes don't leave the sandbox until a human or a policy approves them. For Coder's CDEs, the same layered approach applies at a different scale: the question is which isolation primitive — container, microVM, namespace — gives the right security/performance tradeoff for concurrent coding agents.

**2. "How do you think about supporting multiple AI agent frameworks without building N custom integrations?"**

Don't make them learn your API. Implement their interface. qortex ships adapters for 7 agent frameworks, and the design principle is: one import swap. If you're using CrewAI, you import qortex's CrewAI tool instead of CrewAI's default retrieval, and everything else works — same method signatures, same return types, same error handling. We pass CrewAI's own test suite, not ours. For Coder, the same principle applies: a CDE for autonomous agents should expose the interfaces agents already expect (filesystem, terminal, MCP), not force each agent framework to build a Coder-specific integration.

**3. "You don't know Go. How would you ramp up?"**

I switch between Python and TypeScript daily and I'm actively learning Rust. Go is syntactically simpler with a more constrained type system. The concurrency model — goroutines and channels — maps directly to the async agent patterns I've already built in Python (asyncio) and TypeScript. I'd expect to be writing production Go within the first few weeks. The bigger ramp is understanding Coder's codebase, architecture decisions, and deployment patterns — not the language syntax.

**4. "What does monitoring autonomous agents look like?"**

Every agent action becomes an OTel span with structured attributes: what tool was called, what arguments were passed, what the result was, how long it took, whether it succeeded. Grafana shows aggregates — P50/P99 latency, tool call success rates, agent session duration. Jaeger shows individual traces — you can follow one agent session from start to finish and see every decision it made. For Coder, when an autonomous coding agent is running in a CDE, the developer and the platform both need to know what it's doing, whether it's stuck, and whether it's doing something dangerous. That requires instrumentation at the CDE layer, not just the agent layer.

**5. "Tell us about your open-source engineering practices."**

CI runs against latest upstream framework versions with a zero-skip policy. If CrewAI ships a breaking change on Monday, our CI fails on Monday — not when a user reports it three weeks later. Both qortex and bilrost are published on PyPI with proper packaging. Documentation lives on GitHub Pages via mkdocs. The principle: treat your own tools with the same rigor you'd expect from the tools you depend on. I've maintained 7 framework adapters against upstream breaking changes, which taught me what real open-source maintenance demands — you don't just ship, you keep shipping.

### 30-Second "Tell Me About Yourself" (Tailored to Coder)

"I build infrastructure that makes autonomous agents safe to run in development environments. My main project is a hardened VM sandbox called bilrost — Lima VM, per-tool network isolation, gated filesystem sync — because agents need containment before they need capabilities. Around that, I've built an agent runtime, a full OTel observability pipeline, an MCP server, and adapters for 7 agent frameworks. Everything is published on PyPI with CI that breaks the same day a dependency ships a breaking change. Coder is solving the same containment and integration problem I've been solving, but at the scale of thousands of developers and their coding agents. That's the scale I want to work at."

### One Question to Ask Them

"Coder's CDEs are now hosting autonomous coding agents alongside human developers. How does the team think about the isolation boundary between an agent's actions and the rest of the development environment — is the agent a peer to the developer inside the CDE, or does it get its own sandboxed context within the environment?"

---

## Location Note

Remote role. Currently in Syracuse, NY (temporary); home base is Atlanta, GA. Fully available for US/Canada time zones. Happy to travel for team events, offsites, and onboarding.
