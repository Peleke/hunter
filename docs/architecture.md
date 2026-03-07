# Architecture

Hunter is a chain of Claude Code skills connected by a typed envelope contract, writing structured output to an Obsidian vault.

## Core Concepts

### PipelineEnvelope

Every skill wraps its output in a `PipelineEnvelope`:

```json
{
  "skill": "signal-scan",
  "version": "1.0",
  "session_id": "session-2026-03-06-001",
  "timestamp": "2026-03-06T14:30:00Z",
  "input_refs": [],
  "output": { ... }
}
```

| Field | Type | Purpose |
|-------|------|---------|
| `skill` | enum | Which skill produced this output (15 valid values) |
| `version` | `"1.0"` | Schema version for forward compatibility |
| `session_id` | string | Links artifacts from the same pipeline run |
| `timestamp` | ISO 8601 | When the output was produced |
| `input_refs` | string[] | Filename slugs of upstream artifacts. Empty for `signal-scan` (first in chain). |
| `output` | object | Skill-specific structured JSON (minimum 1 property) |

The schema lives at `schemas/pipeline-envelope.schema.json`. The eval framework (7b-e) validates envelopes against it.

### Vault-as-Database

The Obsidian vault is the primary data store. The mapping is direct:

| Database Concept | Vault Equivalent |
|-----------------|------------------|
| Table | Folder (`Admin/Product-Discovery/Decisions/`) |
| Row | Markdown file with YAML frontmatter |
| Column | Frontmatter field (`type`, `date`, `status`, `tags`) |
| Foreign key | Wikilink (`[[decision-ref]]`) or `*_ref` frontmatter field |
| Index/View | Dataview query in `_index.md` |
| Status column | Kanban board column |
| SQL | Dataview DQL |

This is intentionally designed for mechanical migration to a real backend when the time comes. Every folder becomes a table, every frontmatter field becomes a column, every wikilink becomes a foreign key.

### Folder Contract

Each document type maps to exactly one folder:

```
${VAULT}/Admin/Product-Discovery/
├── Pipeline.kanban.md       # Master board (7 columns)
├── _index.md                # Dataview queries
├── Signal-Scans/            # signal-scan output
├── Decisions/               # decision-log output
├── Personas/                # persona-extract output
├── SWOT/                    # swot-analysis output
├── Offers/                  # offer-scope output
├── Pitches/                 # pitch output + launch kanbans
├── Sessions/                # session logs
├── One-Pagers/              # one-pager output
└── Landing-Pages/           # landing-page output
```

Filename format: `{descriptive-slug}-{YYYY-MM-DD}.md`. If a file already exists at the target path, skills append under `## Update: {ISO timestamp}` rather than overwriting.

---

## Pipeline Data Flow

```
signal-scan ──> decision-log ──> persona-extract ──> offer-scope ──> pitch ──> hunter-log
                                      │                                │
                                      ├──> swot-analysis (optional)    ├──> slidev-deck
                                      │                                ├──> landing-page
                                      │                                ├──> one-pager
                                      │                                ├──> design-pass
                                      │                                └──> narrative-pass
                                      │
                                      └──> content-planner
                                                │
                                          (reads pipelines
                                           + GitHub + buildlog
                                           + wild-scan indices)

wild-scan ──> Writing/From-The-Wild/ (quote indices)
         └──> Writing/Content-Briefs/ (series: "From the Wild")
```

Each arrow is a `PipelineEnvelope` handoff. The `input_refs` field in each envelope points back to the upstream artifact that produced the data it consumes.

### Skill Categories

| Category | Directory | Purpose |
|----------|-----------|---------|
| **Core** | `skills/core/` | The main pipeline chain: scan, decide, persona, SWOT, scope, pitch |
| **Support** | `skills/support/` | Output rendering + persistence: decks, pages, one-pagers, design, narrative, vault I/O |
| **Research** | `skills/research/` | Data acquisition: Reddit scraping, wild-scan quote harvesting, quote-to-persona bridging |
| **Content** | `skills/content/` | Content planning and LinkedIn distribution via LinWheel |
| **Community** | `skills/community/` | Community platform design: paid communities, Skool integration |

### Skill Execution Contract

Every skill:

1. Reads `skills/_conventions.md` before doing anything
2. Resolves paths from `.hunter-config.yaml` (falling back to convention defaults)
3. Produces JSON output wrapped in a `PipelineEnvelope`
4. Calls `hunter-log` to persist to the vault (or writes directly following conventions)
5. Links backward to upstream artifacts via `*_ref` fields in frontmatter
6. Updates `Pipeline.kanban.md` if the skill advances the pipeline stage

### Context Forking

Skills that perform extensive web search declare `context: fork` in their frontmatter. This tells Claude Code to run them in a subagent to avoid polluting the main context window with search results.

| Forks | Does Not Fork |
|-------|---------------|
| signal-scan, decision-log, persona-extract, swot-analysis, reddit-harvest | offer-scope, pitch, landing-page, hunter-log, one-pager, design-pass |

---

## Prose Quality Gate (Bragi)

Any skill that produces human-facing prose runs through the Bragi reviewer before shipping. Bragi is a 20-rule prose quality persona loaded via `buildlog_gauntlet_rules(persona="bragi")`.

The rules catch LLM-sounding patterns: em-dash overuse, AI vocabulary (pivotal, showcase, tapestry, vibrant), inflated significance framing, promotional puffery, performative honesty, tricolon defaults, and self-referential pivots.

Skills that must pass Bragi: `pitch`, `community-pitch`, `skool-pitch`, `content-planner`, `narrative-pass`, and any future skill that generates prose for human consumption.

The reviewer persona lives at `reviewers/bragi.md`.

---

## Kanban Contract

The pipeline kanban at `Pipeline.kanban.md` has 7 columns:

| Column | Moved By | Trigger |
|--------|----------|---------|
| Signal Scanned | `signal-scan` | Scan completes |
| Decision Made | `decision-log` | Decision recorded |
| Persona Researched | `persona-extract` | Personas extracted |
| Offer Scoped | `offer-scope` | Offer spec finalized |
| Building | Operator (manual) | Launch date set |
| Shipped | Operator (manual) | Product live |
| Killed | Operator or kill criteria | Kill criteria triggered |

Skills that do NOT move cards: `swot-analysis` (runs between persona and offer, same stage), `pitch` (generates materials but doesn't commit to launch), `landing-page`, `one-pager`, `content-planner`.

---

## Eval Framework (7b-e)

Four levels of validation, each more expensive and less frequent:

| Level | What | Model | When |
|-------|------|-------|------|
| **7b** | Input validation: reject malformed envelopes | Schema (no LLM) | Every PR |
| **7c** | Output conformance: valid envelopes pass schema | Schema (no LLM) | Every PR |
| **7d** | Pipeline integration: full chain connects, refs resolved | Sonnet | Release |
| **7e** | Quality scoring: 5-dimension rubric, LLM-as-judge | Opus | Release |

Pass criteria: 7b/7c require 100%. 7e requires weighted score >= 7.0 with no dimension below 5.0. The 5 dimensions: evidence grounding (3x weight), scoring discipline (2x), prose quality (2x), actionability (2x), cross-ref integrity (1x).

See `evals/README.md` for fixtures, schemas, and run commands.
