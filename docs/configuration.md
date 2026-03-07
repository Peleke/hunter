# Configuration

Hunter Pipeline is configured via `.hunter-config.yaml` at the repo root.

## Reference

```yaml
# Hunter Pipeline Configuration
# Override defaults for your environment

# Obsidian vault root (required)
# Tilde expansion is supported
vault: ~/Library/Mobile Documents/iCloud~md~obsidian/Documents/YourVault

# Pipeline output directories (relative to vault)
paths:
  signal_scans: Admin/Product-Discovery/Signal-Scans
  decisions: Admin/Product-Discovery/Decisions
  personas: Admin/Product-Discovery/Personas
  swot: Admin/Product-Discovery/SWOT
  offers: Admin/Product-Discovery/Offers
  pitches: Admin/Product-Discovery/Pitches
  sessions: Admin/Product-Discovery/Sessions
  content_briefs: Writing/Content-Briefs
  kanban: Admin/Product-Discovery/Pipeline.kanban.md

# Skills directory (optional)
# null = auto-detect from repo root (recommended)
# Set to override if skills are installed elsewhere
skills_dir: null

# Launchpad repo for pitch deployment (optional)
launchpad:
  repo: YourGitHub/launchpad
  branch_prefix: pitch/

# Bragi prose reviewer path (relative to repo root)
reviewer: reviewers/bragi.md

# Pipeline defaults
defaults:
  design_tier: high        # high | medium | full | custom
  include_swot: true       # Run SWOT analysis between persona and offer
  auto_kanban: true        # Auto-move kanban cards on stage completion
  session_tracking: true   # Log session start/end in vault
```

## Fields

### `vault` (required)

Absolute path to your Obsidian vault root. Tilde expansion (`~`) is supported. This should point to the vault root directory, not the `.obsidian/` config directory.

iCloud-synced vaults work. The typical macOS iCloud path:
```
~/Library/Mobile Documents/iCloud~md~obsidian/Documents/YourVaultName
```

### `paths` (optional)

Subdirectories within the vault where each pipeline stage writes its output. All paths are relative to `vault`. The defaults match the standard hunter vault structure. You only need to set these if your vault uses different folder names.

| Key | Default | Used By |
|-----|---------|---------|
| `signal_scans` | `Admin/Product-Discovery/Signal-Scans` | `signal-scan` |
| `decisions` | `Admin/Product-Discovery/Decisions` | `decision-log` |
| `personas` | `Admin/Product-Discovery/Personas` | `persona-extract` |
| `swot` | `Admin/Product-Discovery/SWOT` | `swot-analysis` |
| `offers` | `Admin/Product-Discovery/Offers` | `offer-scope` |
| `pitches` | `Admin/Product-Discovery/Pitches` | `pitch` |
| `sessions` | `Admin/Product-Discovery/Sessions` | `hunter-log` |
| `content_briefs` | `Writing/Content-Briefs` | `content-planner` |
| `kanban` | `Admin/Product-Discovery/Pipeline.kanban.md` | `hunter-log` (card moves) |

### `skills_dir` (optional)

Where hunter skills are installed. Set to `null` (default) for auto-detection from the repo root. Only override this if you install skills in a non-standard location.

### `launchpad` (optional)

GitHub repo and branch prefix for deploying pitch assets (landing pages, decks) via `slidev-deck` and `landing-page` skills.

- `repo`: GitHub `owner/repo` format
- `branch_prefix`: Branch name prefix for deployment branches (default: `pitch/`)

### `reviewer` (optional)

Path to the Bragi prose reviewer persona, relative to the repo root. Used by skills that produce human-facing prose. Default: `reviewers/bragi.md`.

### `defaults` (optional)

| Key | Default | Description |
|-----|---------|-------------|
| `design_tier` | `high` | Design quality level for `design-pass`. Options: `high`, `medium`, `full`, `custom` |
| `include_swot` | `true` | Whether to run `swot-analysis` between `persona-extract` and `offer-scope` |
| `auto_kanban` | `true` | Whether skills auto-move kanban cards when completing a stage |
| `session_tracking` | `true` | Whether to log session start/end timestamps in the vault |

## Resolution Order

Skills resolve paths in this order:

1. **`.hunter-config.yaml`** at the repo root (highest priority)
2. **Environment variables** (`HUNTER_VAULT`, `HUNTER_SKILLS_DIR`)
3. **`_conventions.md` defaults** (lowest priority, hardcoded fallbacks)

## Path Variables

Once resolved, these variables are available to all skills:

| Variable | Source | Meaning |
|----------|--------|---------|
| `${VAULT}` | `vault` config key | Obsidian vault root |
| `${SKILLS_DIR}` | `skills_dir` config key or auto-detect | Skills installation directory |
| `${HUNTER_DIR}` | Always the repo root | Hunter repo root (for internal docs, schemas) |

## Minimal Config

The only required field is `vault`. Everything else has sensible defaults:

```yaml
vault: ~/Library/Mobile Documents/iCloud~md~obsidian/Documents/MyVault
```

## Vault Setup

If you're starting from scratch, create the folder structure in your vault:

```
Admin/
└── Product-Discovery/
    ├── Pipeline.kanban.md
    ├── _index.md
    ├── Signal-Scans/
    ├── Decisions/
    ├── Personas/
    ├── SWOT/
    ├── Offers/
    ├── Pitches/
    └── Sessions/
Writing/
├── Content-Briefs/
├── From-The-Wild/
└── Drafts/
```

Or let the pipeline create folders on first run. Skills will create missing directories automatically.
