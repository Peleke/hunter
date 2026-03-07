<div align="center">

# Hunter Pipeline

### Opportunity-to-Launch in Six Skills

[![Claude Code](https://img.shields.io/badge/Claude_Code-Plugin-4DCFC9?style=for-the-badge&logo=anthropic&logoColor=white)]()
[![Skills](https://img.shields.io/badge/Skills-20-4DCFC9?style=for-the-badge)]()
[![Eval Framework](https://img.shields.io/badge/Evals-7b--e-4ade80?style=for-the-badge)]()
[![License](https://img.shields.io/badge/License-MIT-4ade80?style=for-the-badge)]()

**Signal. Decision. Persona. Offer. Pitch. Ship.**

[Pipeline](#the-pipeline) · [Skills](#skills-catalog) · [Architecture](#architecture) · [Evals](#eval-framework) · [Getting Started](#getting-started)

---

</div>

## The Problem

Most "AI agent" tools automate fragments. Summarize a doc. Draft an email. Generate a landing page. None of them do the full loop: spot an opportunity in the wild, validate it against real evidence, understand who actually hurts, scope what to build, generate every launch asset, and track whether it worked.

Hunter Pipeline does the full loop. Six core skills, chained through a typed envelope contract, writing structured output to an Obsidian vault that doubles as a queryable database. Every claim traced to evidence. Every decision logged with kill criteria. Every asset deployable from the vault.

---

## The Pipeline

```
                                    ┌─────────────────┐
                                    │   wild-scan      │ ─── reddit-harvest
                                    │   (research)     │ ─── quote-to-persona
                                    └────────┬────────┘
                                             │ feeds
                                             ▼
┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│              │    │              │    │              │    │              │    │              │    │              │
│  signal-scan │───▶│ decision-log │───▶│persona-extract│──▶│  offer-scope │───▶│    pitch     │───▶│  hunter-log  │
│              │    │              │    │              │    │              │    │              │    │              │
│   Stage 1    │    │   Stage 2    │    │   Stage 3    │    │   Stage 4    │    │   Stage 5    │    │  Persistence │
│   Signals    │    │   Decide     │    │   Personas   │    │   Scope      │    │   Launch Kit │    │  Vault I/O   │
│              │    │              │    │              │    │              │    │              │    │              │
└──────────────┘    └──────────────┘    └──────┬───────┘    └──────────────┘    └──────┬───────┘    └──────────────┘
                                               │                                      │
                                               ▼                                      ▼
                                        ┌──────────────┐                   ┌────────────────────┐
                                        │swot-analysis │                   │  Output Skills     │
                                        │  (optional)  │                   │                    │
                                        │ stress-test  │                   │  slidev-deck       │
                                        └──────────────┘                   │  landing-page      │
                                                                           │  one-pager         │
                                                                           │  design-pass       │
                                                                           └────────────────────┘
```

Each stage consumes the output of the previous stage via a typed `PipelineEnvelope`. Every skill reads `_conventions.md` before executing, ensuring consistent vault paths, frontmatter schemas, and cross-reference syntax across the entire pipeline.

---

## Skills Catalog

### Core Pipeline (`skills/core/`)

| | Skill | Description | Consumes | Produces |
|---|---|---|---|---|
| 📡 | **[signal-scan](skills/core/signal-scan/)** | Scan a domain for product opportunities across 7 signal types | Domain + operator edge | Scored opportunities + ship candidates |
| ⚖️ | **[decision-log](skills/core/decision-log/)** | Structured go/no-go decision with SDP scoring and pre-mortem | Signal scan output | Decision record + kill criteria |
| 👤 | **[persona-extract](skills/core/persona-extract/)** | Evidence-based buyer personas from real community stories | Decision + signal data | Persona archetypes + Four Forces |
| 🎯 | **[offer-scope](skills/core/offer-scope/)** | 1-day build spec with positioning, pricing, and distribution | Persona + SPEND data | Build spec + revenue model |
| 🚀 | **[pitch](skills/core/pitch/)** | Complete go-to-market launch kit with deployable assets | Offer spec + all upstream | Landing page, posts, emails, deck |
| 📒 | **[hunter-log](skills/support/hunter-log/)** | Persistence layer — vault I/O, kanban updates, session tracking | PipelineEnvelope (any) | Obsidian markdown + kanban card |

### Research (`skills/research/`)

| | Skill | Description |
|---|---|---|
| 🎯 | **[wild-scan](skills/research/wild-scan/)** | Pain-quote harvester for the "From the Wild" content series |
| 📡 | **[reddit-harvest](skills/research/reddit-harvest/)** | Playwright-driven Reddit quote harvester with full metadata |
| 🔬 | **[quote-to-persona](skills/research/quote-to-persona/)** | Bridge wild-scan quotes into persona-extract-compatible archetypes |

### Output & Deployment (`skills/support/`)

| | Skill | Description |
|---|---|---|
| 🎬 | **[slidev-deck](skills/support/slidev-deck/)** | Slidev presentations with inline hand-drawn SVGs |
| 🌐 | **[landing-page](skills/support/landing-page/)** | Single-file HTML landing page from pitch copy, zero deps |
| 📄 | **[one-pager](skills/support/one-pager/)** | Print-ready one-pager as React JSX, exported to PDF |
| 🎨 | **[design-pass](skills/support/design-pass/)** | Tiered aesthetic upgrades (CSS/SVG) for decks, pages, one-pagers |

### Content & Distribution (`skills/content/`)

| | Skill | Description |
|---|---|---|
| 📰 | **[content-planner](skills/content/content-planner/)** | Scan GitHub + buildlog, generate Obsidian content plan |
| 🎡 | **[linwheel-content-engine](skills/content/linwheel-content-engine/)** | Drive LinWheel LinkedIn content pipeline via MCP tools |
| 🎯 | **[linwheel-source-optimizer](skills/content/linwheel-source-optimizer/)** | Optimize vault notes for LinWheel reshape quality |

### Community (`skills/community/`)

| | Skill | Description |
|---|---|---|
| 🏘️ | **[community-pitch](skills/community/community-pitch/)** | Paid community plan with tiers, cadence, and economics |
| 🎓 | **[skool-pitch](skills/community/skool-pitch/)** | Skool-specific community design with gamification and launch playbook |

---

## Architecture

### PipelineEnvelope

Every skill wraps its output in a typed envelope:

```json
{
  "skill": "signal-scan",
  "version": "1.0",
  "session_id": "session-2026-03-06-001",
  "timestamp": "2026-03-06T14:30:00Z",
  "input_refs": ["upstream-artifact-id"],
  "output": { }
}
```

### Vault-as-Database

The Obsidian vault is structured as a queryable database:

```
Admin/Product-Discovery/
├── Pipeline.kanban.md          # Master board: 7 columns
├── _index.md                   # Dataview queries
├── Signal-Scans/               # signal-scan output
├── Decisions/                  # decision-log output
├── Personas/                   # persona-extract output
├── SWOT/                       # swot-analysis output
├── Offers/                     # offer-scope output
├── Pitches/                    # pitch output + launch kanbans
├── Sessions/                   # session logs
└── Pipeline.md                 # Product tracker table
```

- **Folders** = tables
- **Frontmatter** = schema (type, date, status, tags, refs)
- **Wikilinks** = foreign keys
- **Dataview** = SQL
- **Kanban boards** = status columns

### Configuration

Pipeline paths and defaults are configured in `.hunter-config.yaml` at the repo root. Skills resolve paths from this config, falling back to `_conventions.md` defaults.

---

## Eval Framework

Four-level evaluation system (7b-e) for pipeline quality assurance:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  7b: Input  │───▶│ 7c: Output  │───▶│7d: Pipeline │───▶│ 7e: Quality │
│  Validation │    │ Conformance │    │ Integration │    │  Scoring    │
│             │    │             │    │             │    │             │
│  Bad inputs │    │  Schema     │    │ Full chain  │    │ LLM-as-     │
│  rejected?  │    │  valid?     │    │ connected?  │    │ judge       │
│             │    │             │    │             │    │             │
│  Haiku      │    │  Haiku      │    │  Sonnet     │    │  Opus       │
│  Every PR   │    │  Every PR   │    │  Release    │    │  Release    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
```

| Level | What | Pass Criteria | Cost | Frequency |
|-------|------|--------------|------|-----------|
| **7b** | Input validation | 100% rejection of bad envelopes | Low | Every PR |
| **7c** | Output conformance | 100% schema validation | Low | Every PR |
| **7d** | Pipeline integration | Full chain completes, refs connected | ~$10 | Release |
| **7e** | Quality scoring | Overall ≥7.0, no dimension <5.0 | ~$15 | Release |

See [`evals/README.md`](evals/README.md) for implementation details.

---

## Repo Layout

```
hunter/
├── skills/
│   ├── _conventions.md        # Pipeline constitution
│   ├── core/                  # 6 core pipeline skills
│   ├── support/               # Output + persistence skills
│   ├── research/              # Research + harvesting skills
│   ├── content/               # Content + distribution skills
│   └── community/             # Community platform skills
├── reviewers/
│   └── bragi.md               # Prose quality reviewer persona
├── schemas/                   # JSON Schema for PipelineEnvelope
├── evals/                     # 7b-e eval framework
├── examples/                  # Example pipeline runs
├── docs/                      # Architecture + quickstart guides
├── buildlog/                  # Session journals
├── upstream/                  # Non-hunter skills (preserved from monorepo)
├── .hunter-config.yaml        # Pipeline configuration
└── README.md
```

---

## Getting Started

### Prerequisites

- [Claude Code](https://claude.com/claude-code) with skills support
- An Obsidian vault (iCloud-synced recommended)
- GitHub CLI (`gh`) for pitch deployment

### Install

```bash
git clone https://github.com/Peleke/hunter.git
```

### Configure

Edit `.hunter-config.yaml` with your vault path:

```yaml
vault: ~/path/to/your/obsidian/vault
```

### Run the Pipeline

```
"Scan DevOps education for product signals"     → signal-scan
"Which opportunity should I pursue?"            → decision-log
"Extract personas for this opportunity"         → persona-extract
"Scope an offer for the top persona"            → offer-scope
"Build the launch kit"                          → pitch
```

Each skill chains automatically. Or run any skill standalone with the right trigger phrase.

---

<div align="center">

**Built for the engineer who can build anything but never had a system for deciding what to build.**

[![GitHub](https://img.shields.io/badge/GitHub-Peleke/hunter-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Peleke/hunter)

</div>
