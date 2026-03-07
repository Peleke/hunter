# Build Journal: Hunter Pipeline Ejection — Phase 1

**Date:** 2026-03-06
**Duration:** ~2 hours (ongoing)
**Status:** In Progress

---

## The Goal

Eject 20 hunter pipeline skills from the `Peleke/skills` monorepo into a standalone `Peleke/hunter` repo. This matters for three reasons:

1. **Product credibility** — the pipeline is the product; it needs its own repo, README, and identity
2. **Plugin packaging** — Claude Code marketplace packaging requires a standalone repo with `.claude-plugin/` structure
3. **Eval framework** — the 7b-e eval system needs its own CI pipeline, not buried in a skills monorepo

The pipeline has been validated through dogfooding — it generated the ACE validation deck, including running on itself recursively. Now it needs to stand alone as a shippable artifact.

---

## What We Built

### Architecture

```
Peleke/skills (monorepo)
├── skills/custom/                    ← 29 skills, mixed concerns
│   ├── signal-scan/                  ← hunter pipeline
│   ├── interview-prep/               ← personal tooling
│   └── ...
│
└── git subtree split --prefix=skills/custom/ -b hunter-extract
    │
    ▼
Peleke/hunter (standalone)
├── skills/
│   ├── core/         (6)   signal-scan, decision-log, persona-extract,
│   │                       swot-analysis, offer-scope, pitch
│   ├── support/      (5)   hunter-log, design-pass, slidev-deck,
│   │                       landing-page, one-pager
│   ├── research/     (3)   reddit-harvest, wild-scan, quote-to-persona
│   ├── content/      (3)   content-planner, linwheel-content-engine,
│   │                       linwheel-source-optimizer
│   ├── community/    (2)   community-pitch, skool-pitch
│   └── _conventions.md     pipeline constitution
├── reviewers/                bragi.md prose reviewer
├── schemas/                  PipelineEnvelope JSON Schema
├── evals/                    7b-e eval framework
├── upstream/                 9 non-hunter skills (preserved, not deleted)
├── .hunter-config.yaml       pipeline configuration
└── README.md                 product documentation
```

### Components

| Component | Status | Notes |
|-----------|--------|-------|
| Subtree split (68 commits) | Complete | Full git history preserved for portfolio credibility |
| Directory restructure | Complete | 5 category dirs under skills/ |
| Non-hunter skill preservation | Complete | Moved to upstream/, not deleted |
| README.md | Complete | Product-grade with ASCII pipeline diagram, eval framework, repo layout |
| .hunter-config.yaml | Complete | Vault paths, defaults, launchpad integration |
| bragi.md reviewer | Complete | Copied from skills/claude/ |
| _conventions.md | Complete | Moved to skills/ root |
| docs/working/ | Complete | Existing research docs relocated |
| GitHub repo + epic issue | Complete | Peleke/hunter #1 with full 6-phase plan |

---

## The Journey

### Phase 1A: Subtree Split

**What we tried:** `git subtree split --prefix=skills/custom/ -b hunter-extract` in the skills monorepo to extract all custom skills with full commit history.

**What happened:** Clean extraction of 68 commits into a `hunter-extract` branch. Every commit that touched `skills/custom/` was preserved with original authorship and timestamps.

**Why this matters:** The subtree split technique preserves git blame and authorship across repo boundaries. For a portfolio project where demonstrating sustained engineering work matters, this is critical. A flat `cp -r` would show one commit with everything appearing on the same day. The subtree split shows the real development history — 68 commits of iterative refinement.

**Technique:**
```bash
# In the source monorepo (Peleke/skills):
git subtree split --prefix=skills/custom/ -b hunter-extract

# In the target repo (Peleke/hunter):
git remote add skills-extract /path/to/skills
git fetch skills-extract hunter-extract
git merge skills-extract/hunter-extract --allow-unrelated-histories
```

**Lesson:** `git subtree split` is the cleanest way to extract a subdirectory into its own repo when you care about history. It rewrites commit SHAs but preserves authorship, timestamps, and diffs. Works better than `git filter-branch` for modern use cases.

### Phase 1B: Directory Restructure

**What we tried:** Reorganize the flat skill directory (29 items at one level) into 5 categorized directories matching the product's information architecture.

**What happened:** Clean restructure. The user correctly caught that non-hunter skills should be preserved, not deleted — moved to `upstream/` instead. This maintains the complete monorepo snapshot for reference while clearly separating product skills from personal tooling.

**Lesson:** When ejecting a subset from a monorepo, don't delete what you're not taking. Move it to a clearly-labeled holding area. The original monorepo still has the canonical copy; the holding area is for reference and prevents "wait, where did X go?" confusion.

### Phase 1C: Configuration + Documentation

**What we tried:** Create `.hunter-config.yaml` for pipeline configuration and a product-grade README.md.

**What happened:** Config file provides vault paths, pipeline defaults, and launchpad integration. README includes the full ASCII pipeline diagram, skills catalog with correct paths, eval framework overview, and repo layout map.

**Lesson:** The README is the product's front door. For a Claude Code plugin that will be discovered through marketplace search, the README has to immediately communicate: what it does, how it works, and how to run it.

---

## What's Left

- [ ] Create `.gitignore` for hunter repo
- [ ] Initial commit + push to origin
- [ ] Phase 2: Config extraction — update _conventions.md with config variable resolution
- [ ] Phase 3: Plugin packaging — `.claude-plugin/marketplace.json` with 5 modular plugins
- [ ] Phase 4: Eval framework — 7b-e implementation with fixtures and CI
- [ ] Phase 5: Documentation — architecture.md, configuration.md, quickstart.md
- [ ] Phase 6: Launch — v1.0.0 tag, skills monorepo cleanup, announcement

---

## Improvements

### Architectural

- Subtree split preserves authorship across repo boundaries — use this pattern for any monorepo ejection
- Category-based directory structure (core/support/research/content/community) maps cleanly to plugin sub-packages
- `upstream/` directory pattern for preserving non-extracted content without deletion

### Workflow

- Started buildlog entry at session start this time (learned from the ACE deck session where we were 4 hours in before logging)
- Epic issue created on GitHub before starting work — tracks all 6 phases with checkboxes
- Buildlog experiment session tracking active for the full ejection batch

### Tool Usage

- `git subtree split --prefix=X -b branch` — extract subdirectory with full history
- `git remote add + fetch + merge --allow-unrelated-histories` — import extracted history into new repo
- `mkdir -p` with full path creates nested category structure in one command

### Domain Knowledge

- Git subtree split rewrites SHAs but preserves authorship/timestamps/diffs
- `--allow-unrelated-histories` is required when merging a subtree-split branch into a fresh repo
- Claude Code plugin packaging expects `.claude-plugin/marketplace.json` at repo root

---

## Agent-Facing Notes

### For Future Pipeline Runs

When the hunter pipeline runs in this repo (dogfooding), resolve skill paths from `.hunter-config.yaml` → `skills_dir` field. If null, auto-detect from the repo root: `${REPO_ROOT}/skills/`.

### For Eval Authors

The 7b-e eval framework lives in `evals/`. Each level has its own directory:
- `evals/7b-input/` — invalid envelope fixtures (expect rejection)
- `evals/7c-output/` — valid input fixtures + JSON Schema validators
- `evals/7d-pipeline/` — full-chain integration test harnesses
- `evals/7e-quality/` — LLM-as-judge rubrics + calibration data

Run order matters: 7b and 7c gate PRs (cheap, fast). 7d and 7e run on release (expensive, comprehensive).

### For Content Generation

This buildlog entry is source material for:
- **Article 3** (Recursive Proof) — the subtree split technique and dogfooding narrative
- **LinkedIn post** — "Your AI agent skills are stuck in a monorepo. Here's how to eject them into a product."
- **buildlog-to-content pipeline** — this entry feeds the LinWheel content engine via `content-planner`

### Key Decisions Log

| Decision | Why | Alternative Considered |
|----------|-----|----------------------|
| Subtree split (not flat copy) | Preserves git history for portfolio credibility | `cp -r` — faster but loses all history |
| upstream/ (not delete) | User preference: don't destroy, relocate | `rm -rf` — cleaner but irreversible |
| Category dirs (not flat) | Maps to plugin sub-packages, improves discoverability | Flat list — simpler but 20 skills in one dir is chaos |
| .hunter-config.yaml (not env vars) | Checked into repo, self-documenting, YAML for complex structure | `.env` — simpler but not structured, not version-controlled |

---

## Files Changed

```
hunter/
├── README.md                           # Product documentation
├── .hunter-config.yaml                 # Pipeline configuration
├── skills/
│   ├── _conventions.md                 # Pipeline constitution (moved from root)
│   ├── core/                           # 6 core pipeline skills
│   │   ├── signal-scan/
│   │   ├── decision-log/
│   │   ├── persona-extract/
│   │   ├── swot-analysis/
│   │   ├── offer-scope/
│   │   └── pitch/
│   ├── support/                        # 5 output + persistence skills
│   │   ├── hunter-log/
│   │   ├── design-pass/
│   │   ├── slidev-deck/
│   │   ├── landing-page/
│   │   └── one-pager/
│   ├── research/                       # 3 research skills
│   │   ├── reddit-harvest/
│   │   ├── wild-scan/
│   │   └── quote-to-persona/
│   ├── content/                        # 3 content skills
│   │   ├── content-planner/
│   │   ├── linwheel-content-engine/
│   │   └── linwheel-source-optimizer/
│   └── community/                      # 2 community skills
│       ├── community-pitch/
│       └── skool-pitch/
├── reviewers/
│   └── bragi.md                        # Prose reviewer (copied from skills/claude/)
├── upstream/                           # 9 non-hunter skills (preserved)
├── schemas/                            # PipelineEnvelope JSON Schema (pending)
├── evals/                              # 7b-e eval framework (pending)
├── examples/                           # Example pipeline runs (pending)
├── docs/
│   └── working/                        # Existing research docs (relocated)
└── buildlog/
    └── 2026-03-06-hunter-ejection.md   # This entry
```

---

## AI Experience Reflection — buildlog Tools

### What Worked Well

- **`buildlog_experiment_start` + `buildlog_commit` loop**: Once the experiment session was active, `buildlog_commit` wrapped git commits cleanly and auto-appended the commit block to the entry. The auto-logging of file lists is genuinely useful — I don't have to manually track what changed.
- **Hook enforcement**: The pre-commit hook that blocks `git commit` and forces you through `buildlog_commit` is effective. It caught me twice in this session (once in the portfolio repo, once here). Without it, I would have used bare `git commit` every time and the buildlog would be empty.
- **Entry template**: The `_TEMPLATE.md` provided good structure. Having the sections pre-defined (Goal, Architecture, Journey, Improvements) made it easy to fill in as work progressed rather than trying to reconstruct at the end.

### What Was Frustrating

- **"No active experiment session" error**: Every fresh repo requires `buildlog_experiment_start` before `buildlog_commit` works, but the error message doesn't appear until you try to commit. This means the first commit attempt always fails in a new repo. Suggestion: either auto-start a session on first commit, or have `buildlog init` create a default session.
- **Session ID opacity**: The session ID (`session-20260307-010145-825714`) is auto-generated and never referenced again. I don't know how to resume it, query it, or correlate it with the buildlog entry. The connection between sessions and entries should be more explicit.
- **`buildlog_dir` path resolution**: Had to pass the full absolute path (`/Users/peleke/Documents/Projects/hunter/buildlog`) because the tool doesn't auto-detect the buildlog directory from the current git root. When working across multiple repos in one session, this means remembering to switch the `buildlog_dir` param each time.
- **Commit file list verbosity**: The auto-appended commit block lists every file, which for a 200+ file initial commit produces a wall of text truncated with "...and 206 more". A summary (e.g., "226 files across skills/, upstream/, docs/, buildlog/") would be more useful for large commits.

### Communication Notes

- The buildlog MCP tools feel like infrastructure plumbing — reliable but invisible. The value is in the *output* (entries, reward signals, skills extraction), not the tool UX itself. That's fine for a data capture layer.
- The biggest risk is forgetting to start the experiment session. This happened in the previous session too (ACE deck). Consider making this the default state rather than an opt-in.

### Suggestions for buildlog Tool Improvements

1. **Auto-session on first commit**: If no session is active, start one automatically with the branch name as context
2. **Smart file list summarization**: For commits with >20 files, group by directory and show counts instead of listing every file
3. **`buildlog_dir` auto-detection**: Walk up from `cwd` to find the nearest `buildlog/` directory, like git does for `.git/`
4. **Session-entry linking**: Auto-add the session ID to the entry frontmatter so sessions and entries are correlated
5. **Entry slug from branch**: When `slug` isn't provided, derive it from the current branch name (we already do this but it could be more reliable across repos)

---

*Next entry: Phase 2-3 config extraction + plugin packaging*

## Commits

### `e943b21` — feat: eject hunter pipeline from skills monorepo — Phase 1 complete

Files:
- `.claude/settings.json`
- `.gitignore`
- `.hunter-config.yaml`
- `README.md`
- `_conventions.md`
- `buildlog/.buildlog/.gitkeep`
- `buildlog/.buildlog/seeds/.gitkeep`
- `buildlog/.gitkeep`
- `buildlog/2026-01-01-example.md`
- `buildlog/2026-03-06-hunter-ejection.md`
- `buildlog/BUILDLOG_SYSTEM.md`
- `buildlog/_TEMPLATE.md`
- `buildlog/_TEMPLATE_QUICK.md`
- `buildlog/assets/.gitkeep`
- `chapter-generator/SKILL.md`
- `chapter-generator/reference/voice-guide.md`
- `community-pitch/SKILL.md`
- `community-pitch/references/output-schema.json`
- `content-planner/SKILL.md`
- `content-planner/scripts/scan_repos.py`
- ...and 206 more


### `5a19e94` — docs: add AI experience reflection + buildlog tool feedback to ejection entry

Files:
- `buildlog/2026-03-06-hunter-ejection.md`


### `8318429` — feat: Phase 2 config extraction — resolve paths from .hunter-config.yaml

Files:
- `buildlog/2026-03-06-hunter-ejection.md`
- `skills/_conventions.md`
- `skills/content/linwheel-source-optimizer/SKILL.md`


### `f855e28` — feat: Phase 3-4 — plugin packaging + eval framework (7b-e)

Files:
- `.claude-plugin/marketplace.json`
- `buildlog/2026-03-06-hunter-ejection.md`
- `evals/7b-input/fixtures/invalid-envelopes.json`
- `evals/7c-output/fixtures/valid-envelopes.json`
- `evals/7e-quality/rubrics/pipeline-quality.md`
- `evals/README.md`
- `schemas/pipeline-envelope.schema.json`


### `1cc26dc` — feat: add Phase 4.5 Narrative Layer to slidev-deck skill

Files:
- `buildlog/2026-03-06-hunter-ejection.md`
- `skills/support/slidev-deck/SKILL.md`


### `b13f733` — feat: deep rewrite Phase 4.5 Narrative Layer — persona-anchored SPIN storytelling

Files:
- `buildlog/2026-03-06-hunter-ejection.md`
- `skills/support/slidev-deck/SKILL.md`


### `f033ddd` — feat: standalone narrative-pass skill with bragi prose gate

Files:
- `buildlog/2026-03-06-hunter-ejection.md`
- `skills/support/narrative-pass/README.md`
- `skills/support/narrative-pass/SKILL.md`


### `4518864` — docs: Phase 5 — architecture, configuration, quickstart + OpenClaw compliance

Files:
- `.claude-plugin/marketplace.json`
- `README.md`
- `docs/architecture.md`
- `docs/configuration.md`
- `docs/quickstart.md`
- `skills/support/design-pass/SKILL.md`

