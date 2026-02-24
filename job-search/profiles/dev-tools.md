# Developer Tools / DevEx Profile

## One-liner
Developer tools engineer who ships SDKs, MCP servers, and framework adapters that make complex systems feel simple.

## Pitch (2-3 sentences)
I build the interfaces between powerful systems and the developers who use them. qortex ships adapters for 7 agent frameworks, each implementing the framework's native interface so users swap one import and gain graph retrieval + a feedback loop. The MCP server bridges Python and TypeScript ecosystems over stdio transport, and bilrost provisions a hardened agent sandbox from a single CLI command.

## Resume emphasis order
1. Framework adapters — native interface implementation, CI conformance testing
2. MCP server — cross-language transport, tool schemas, stdio bridge
3. bilrost CLI — `bilrost up/status/ssh`, profile-based management, PyPI distribution
4. buildlog — gauntlet review system, git hooks, developer workflow integration
5. ComfyUI MCP — image generation via MCP tools for agent workflows
6. Open-source practice — PyPI publishing, docs sites, CI/CD

## Proof points
- 7 framework adapters, all passing framework-authored tests
- Published on PyPI: qortex, bilrost
- MCP server: 29 tool calls over real stdio in 3.94s
- bilrost: single-command provisioning, profile-based config
- CI: latest-version framework pinning, zero-skip policy

## "If they ask about X, talk about Y"
- **"How do you think about DX?"** → Users shouldn't learn a new API. Implement the interface they already use. Pass the tests they already wrote. One import swap.
- **"How do you handle cross-language?"** → MCP stdio transport. Same Python engine, one transport layer. Suboptimal (distributed service in progress), but for agent workflows where an LLM call is 500ms-5s, stdio overhead doesn't dominate.
- **"Open source experience?"** → PyPI packages, mkdocs sites on GitHub Pages, CI that installs latest framework versions (not pinned). If CrewAI ships a breaking change, we know the same day.

## Target companies
Vercel, Anthropic, Cursor/Anysphere, Replit, Supabase, Clerk, Prisma, Dagger
