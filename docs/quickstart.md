# Quickstart

Get from zero to your first pipeline run in under 5 minutes.

## Prerequisites

- [Claude Code](https://claude.com/claude-code) with skills support
- An Obsidian vault (any vault works; iCloud-synced recommended for mobile access)
- GitHub CLI (`gh`) installed and authenticated (only needed for pitch deployment)

## 1. Clone

```bash
git clone https://github.com/Peleke/hunter.git
cd hunter
```

## 2. Configure

Edit `.hunter-config.yaml` with your vault path:

```yaml
vault: ~/Library/Mobile Documents/iCloud~md~obsidian/Documents/YourVault
```

That's the only required field. See [configuration.md](configuration.md) for full reference.

## 3. Run Your First Scan

Open Claude Code in the hunter directory and say:

```
Scan the DevOps education market for product signals
```

This triggers `signal-scan`, which will:
- Search across 7 signal types (pain, desire, friction, workaround, willingness-to-pay, underserved, trend)
- Score each opportunity on evidence density
- Write structured output to `${VAULT}/Admin/Product-Discovery/Signal-Scans/`
- Add a card to `Pipeline.kanban.md`

## 4. Chain the Pipeline

Each subsequent skill consumes the previous stage's output:

```
"Which opportunity should I pursue?"            → decision-log
"Extract personas for this opportunity"         → persona-extract
"Run a SWOT on this"                            → swot-analysis (optional)
"Scope an offer for the top persona"            → offer-scope
"Build the launch kit"                          → pitch
```

You can run them one at a time (interactive) or let them chain automatically within a session.

## 5. Generate Launch Assets

After `pitch` runs, use output skills to generate deployable assets:

```
"Build a slide deck for this pitch"             → slidev-deck
"Run a narrative pass on the deck"              → narrative-pass
"Generate a landing page"                       → landing-page
"Create a one-pager PDF"                        → one-pager
"Apply a design pass to the deck"               → design-pass
```

## 6. Persist and Track

Every skill writes to the vault automatically via `hunter-log`. Check your pipeline status:

- **Kanban board**: `Admin/Product-Discovery/Pipeline.kanban.md`
- **Dataview index**: `Admin/Product-Discovery/_index.md`
- **Individual artifacts**: Each folder under `Admin/Product-Discovery/`

## Skill Packages

Hunter is modular. Install what you need:

| Package | Skills | Use Case |
|---------|--------|----------|
| `hunter-core` | signal-scan, decision-log, persona-extract, swot-analysis, offer-scope, pitch | The full pipeline |
| `hunter-deploy` | hunter-log, slidev-deck, landing-page, one-pager, design-pass, narrative-pass | Output and deployment |
| `hunter-research` | reddit-harvest, wild-scan, quote-to-persona | Research and data acquisition |
| `hunter-content` | content-planner, linwheel-content-engine, linwheel-source-optimizer | Content strategy |
| `hunter-community` | community-pitch, skool-pitch | Community platforms |

## Common Workflows

### Full pipeline (interactive)

Run each skill manually, reviewing output between stages. Good for your first run.

### Research-first

Start with `wild-scan` or `reddit-harvest` to collect pain quotes from real communities. Feed these into `quote-to-persona` to bridge into the main pipeline at the persona stage.

### Content from pipeline

After running the core pipeline, use `content-planner` to scan your pipeline artifacts + GitHub activity + buildlog entries and generate a content calendar with briefs.

### Pitch refinement

After `pitch`, iterate with `slidev-deck` + `narrative-pass` + `design-pass` for presentation-ready decks. Use `narrative-pass` to add persona-anchored storytelling with SPIN arc and performance-script speaker notes.

## Next Steps

- [Architecture](architecture.md): how the envelope contract and vault-as-database work
- [Configuration](configuration.md): full `.hunter-config.yaml` reference
- [Eval Framework](../evals/README.md): how pipeline quality is validated
