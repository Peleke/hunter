# Infrastructure / Platform Profile

## One-liner
Platform engineer who built a defense-in-depth sandbox, OTel observability pipeline, and distributed agent runtime from scratch.

## Pitch (2-3 sentences)
I build the infrastructure that makes agent systems safe and observable. bilrost is a hardened VM sandbox with per-tool network isolation, OverlayFS containment, and gated sync — 117 passing tests on the network isolation layer alone. The observability stack (qortex-observe) instruments every retrieval, feedback event, and graph traversal with OpenTelemetry, shipping to Grafana/Jaeger/Memgraph dashboards.

## Resume emphasis order
1. bilrost — Lima VM, Ansible provisioning, UFW firewall, Docker dual-container isolation, gitleaks sync gate
2. qortex-observe — OTel instrumentation, Grafana dashboards, Jaeger traces, Memgraph visualization
3. Vindler — production agent runtime, 90 merged PRs, system integration
4. qortex distributed service (in progress) — REST/GraphQL API layer, storage backends
5. CI/CD — 100+ adapter tests, latest-version framework pinning, zero-skip policy
6. Prior: edX platform engineering

## Proof points
- 117 passing tests on network isolation
- 90 merged PRs across sandbox + agent integration
- STRIDE threat model with 7 analysis documents, 5 PoC exploits
- Dual-container isolation: air-gapped by default, bridge only per-tool
- Ansible-provisioned infrastructure from a single `bilrost up` command
- Published on PyPI as `bilrost`

## "If they ask about X, talk about Y"
- **"Tell me about your infrastructure work"** → bilrost. Defense-in-depth for agent runtimes. Lima VM + OverlayFS + Docker dual-container + gated sync. Each tool gets its own network policy.
- **"How do you think about security?"** → STRIDE threat modeling. I found that OpenClaw's identity layer is an attack surface (workspace files injected verbatim into system prompt). Built walls instead of patches.
- **"How do you monitor systems?"** → OTel everywhere. Every retrieval, every feedback event, every graph traversal. Grafana for metrics, Jaeger for traces, Memgraph for graph visualization.
- **"Scale?"** → Current work is single-node. Active work on distributed qortex service (REST/GraphQL, configurable storage backends). The adapter pattern carries over.

## Target companies
Anthropic, Vercel, Modal, Fly.io, Cloudflare, Railway, Render, Datadog, Grafana Labs
