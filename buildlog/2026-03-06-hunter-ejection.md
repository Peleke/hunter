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

<!-- buildlog:improvements:start -->
## What Improved This Session

**Clean session.** No mistakes were flagged.

**Rules earning confidence**: `design-pass-dogfood:bragi:02959dda`, `design-pass-dogfood:bragi:2e966d45`, `design-pass-dogfood:bragi:41e6758f`, `design-pass-dogfood:bragi:463490f1`, `design-pass-dogfood:bragi:63cd491d`, `design-pass-dogfood:bragi:71344189`, `design-pass-dogfood:bragi:92b40f51`, `design-pass-dogfood:bragi:980ac190`, `design-pass-dogfood:bragi:9ecf1649`, `design-pass-dogfood:bragi:c33015a1`, `design-pass-dogfood:bragi:c651da02`, `design-pass-dogfood:bragi:ce0516df`, `design-pass-dogfood:bragi:d14b1a2d`, `design-pass-dogfood:bragi:d720e228`, `design-pass-dogfood:bragi:e1b5e53a`, `design-pass-dogfood:bragi:e30cf4d5`, `design-pass-dogfood:bragi:e4d6eee0`, `design-pass-dogfood:bragi:e96e9da4`, `design-pass-dogfood:bragi:eddf641e`, `design-pass-dogfood:bragi:f0384f94`, `design-pass-dogfood:qortex_design_patterns:009a418c`, `design-pass-dogfood:qortex_design_patterns:00c3a9ae`, `design-pass-dogfood:qortex_design_patterns:09ae8ef6`, `design-pass-dogfood:qortex_design_patterns:17b9abd6`, `design-pass-dogfood:qortex_design_patterns:1bbbbcd6`, `design-pass-dogfood:qortex_design_patterns:1c48dc87`, `design-pass-dogfood:qortex_design_patterns:20f54d8e`, `design-pass-dogfood:qortex_design_patterns:211f6637`, `design-pass-dogfood:qortex_design_patterns:316b0d61`, `design-pass-dogfood:qortex_design_patterns:48c2bf9e`, `design-pass-dogfood:qortex_design_patterns:52456083`, `design-pass-dogfood:qortex_design_patterns:539713dc`, `design-pass-dogfood:qortex_design_patterns:5421537f`, `design-pass-dogfood:qortex_design_patterns:604af93a`, `design-pass-dogfood:qortex_design_patterns:61562ff8`, `design-pass-dogfood:qortex_design_patterns:66aa7593`, `design-pass-dogfood:qortex_design_patterns:6895e4e3`, `design-pass-dogfood:qortex_design_patterns:77939dc5`, `design-pass-dogfood:qortex_design_patterns:7c32f1ce`, `design-pass-dogfood:qortex_design_patterns:95639de9`, `design-pass-dogfood:qortex_design_patterns:99cb1875`, `design-pass-dogfood:qortex_design_patterns:9e4ebfc8`, `design-pass-dogfood:qortex_design_patterns:af6223e4`, `design-pass-dogfood:qortex_design_patterns:bf779406`, `design-pass-dogfood:qortex_design_patterns:c07590d6`, `design-pass-dogfood:qortex_design_patterns:c1fd948b`, `design-pass-dogfood:qortex_design_patterns:c9566580`, `design-pass-dogfood:qortex_design_patterns:d1c0e664`, `design-pass-dogfood:qortex_design_patterns:d27ed782`, `design-pass-dogfood:qortex_design_patterns:d4f6ab83`, `design-pass-dogfood:qortex_design_patterns:ec1ddd87`, `design-pass-dogfood:qortex_design_patterns:f0aa24dd`, `design-pass-dogfood:qortex_impl_hiding:009a418c`, `design-pass-dogfood:qortex_impl_hiding:0ae292fe`, `design-pass-dogfood:qortex_impl_hiding:103c3fbf`, `design-pass-dogfood:qortex_impl_hiding:105cfe9f`, `design-pass-dogfood:qortex_impl_hiding:17b9abd6`, `design-pass-dogfood:qortex_impl_hiding:1bbbbcd6`, `design-pass-dogfood:qortex_impl_hiding:29c3da41`, `design-pass-dogfood:qortex_impl_hiding:2bf055c6`, `design-pass-dogfood:qortex_impl_hiding:2e300d8d`, `design-pass-dogfood:qortex_impl_hiding:42e88397`, `design-pass-dogfood:qortex_impl_hiding:44f68a58`, `design-pass-dogfood:qortex_impl_hiding:48c2bf9e`, `design-pass-dogfood:qortex_impl_hiding:4a9e5b86`, `design-pass-dogfood:qortex_impl_hiding:5ec0adfa`, `design-pass-dogfood:qortex_impl_hiding:6a975854`, `design-pass-dogfood:qortex_impl_hiding:6f219f32`, `design-pass-dogfood:qortex_impl_hiding:7974e474`, `design-pass-dogfood:qortex_impl_hiding:7c955b5d`, `design-pass-dogfood:qortex_impl_hiding:7dbbedeb`, `design-pass-dogfood:qortex_impl_hiding:9403a5c5`, `design-pass-dogfood:qortex_impl_hiding:9dafbed4`, `design-pass-dogfood:qortex_impl_hiding:9df38844`, `design-pass-dogfood:qortex_impl_hiding:a3edcfcd`, `design-pass-dogfood:qortex_impl_hiding:af6223e4`, `design-pass-dogfood:qortex_impl_hiding:c9566580`, `design-pass-dogfood:qortex_impl_hiding:c9c16146`, `design-pass-dogfood:qortex_impl_hiding:ca77185f`, `design-pass-dogfood:qortex_impl_hiding:d27ed782`, `design-pass-dogfood:qortex_impl_hiding:ded0c774`, `design-pass-dogfood:qortex_impl_hiding:e20d0f47`, `design-pass-dogfood:qortex_observer:04545c6d`, `design-pass-dogfood:qortex_observer:06600c9c`, `design-pass-dogfood:qortex_observer:09dfcf92`, `design-pass-dogfood:qortex_observer:0c77a1d2`, `design-pass-dogfood:qortex_observer:0c8ba2f8`, `design-pass-dogfood:qortex_observer:11fa7b98`, `design-pass-dogfood:qortex_observer:174f5a1b`, `design-pass-dogfood:qortex_observer:19c437b4`, `design-pass-dogfood:qortex_observer:19d8ee77`, `design-pass-dogfood:qortex_observer:1dc72bd4`, `design-pass-dogfood:qortex_observer:24343eba`, `design-pass-dogfood:qortex_observer:2743739e`, `design-pass-dogfood:qortex_observer:28f8af35`, `design-pass-dogfood:qortex_observer:2950435f`, `design-pass-dogfood:qortex_observer:2bc32ffe`, `design-pass-dogfood:qortex_observer:2dc10cf4`, `design-pass-dogfood:qortex_observer:2ec2b1ae`, `design-pass-dogfood:qortex_observer:2eebc331`, `design-pass-dogfood:qortex_observer:31d84cb5`, `design-pass-dogfood:qortex_observer:36576d68`, `design-pass-dogfood:qortex_observer:371ae9d9`, `design-pass-dogfood:qortex_observer:38723ff3`, `design-pass-dogfood:qortex_observer:39e88ecd`, `design-pass-dogfood:qortex_observer:3a4fec06`, `design-pass-dogfood:qortex_observer:3fed269e`, `design-pass-dogfood:qortex_observer:40645184`, `design-pass-dogfood:qortex_observer:434d864a`, `design-pass-dogfood:qortex_observer:436378ee`, `design-pass-dogfood:qortex_observer:44130d7d`, `design-pass-dogfood:qortex_observer:4875e202`, `design-pass-dogfood:qortex_observer:48a4251a`, `design-pass-dogfood:qortex_observer:4a0879c4`, `design-pass-dogfood:qortex_observer:4c1c9f4b`, `design-pass-dogfood:qortex_observer:4e97c534`, `design-pass-dogfood:qortex_observer:4e9e5c68`, `design-pass-dogfood:qortex_observer:4ff04b79`, `design-pass-dogfood:qortex_observer:507dc23d`, `design-pass-dogfood:qortex_observer:51783efd`, `design-pass-dogfood:qortex_observer:54ed2186`, `design-pass-dogfood:qortex_observer:59b7e51a`, `design-pass-dogfood:qortex_observer:59deb741`, `design-pass-dogfood:qortex_observer:5b87c2a4`, `design-pass-dogfood:qortex_observer:5bd02b0d`, `design-pass-dogfood:qortex_observer:5cd6a960`, `design-pass-dogfood:qortex_observer:5d7d7338`, `design-pass-dogfood:qortex_observer:5d85527d`, `design-pass-dogfood:qortex_observer:5df8aa0a`, `design-pass-dogfood:qortex_observer:5f14d5c7`, `design-pass-dogfood:qortex_observer:610bcbcf`, `design-pass-dogfood:qortex_observer:61865b50`, `design-pass-dogfood:qortex_observer:64c6645d`, `design-pass-dogfood:qortex_observer:685b1c2e`, `design-pass-dogfood:qortex_observer:6a17181c`, `design-pass-dogfood:qortex_observer:6d945803`, `design-pass-dogfood:qortex_observer:7180acf9`, `design-pass-dogfood:qortex_observer:72bfc672`, `design-pass-dogfood:qortex_observer:76c35779`, `design-pass-dogfood:qortex_observer:78868c08`, `design-pass-dogfood:qortex_observer:78d156fc`, `design-pass-dogfood:qortex_observer:7930bb43`, `design-pass-dogfood:qortex_observer:7b88ef97`, `design-pass-dogfood:qortex_observer:7dd1e3da`, `design-pass-dogfood:qortex_observer:821a35c2`, `design-pass-dogfood:qortex_observer:856a93fc`, `design-pass-dogfood:qortex_observer:882600a9`, `design-pass-dogfood:qortex_observer:884e7f55`, `design-pass-dogfood:qortex_observer:888dd5da`, `design-pass-dogfood:qortex_observer:8fe1f256`, `design-pass-dogfood:qortex_observer:9042bdf1`, `design-pass-dogfood:qortex_observer:90765dde`, `design-pass-dogfood:qortex_observer:93dcbbb5`, `design-pass-dogfood:qortex_observer:961a4140`, `design-pass-dogfood:qortex_observer:964c28c2`, `design-pass-dogfood:qortex_observer:98b12a07`, `design-pass-dogfood:qortex_observer:9acc67a4`, `design-pass-dogfood:qortex_observer:9c164033`, `design-pass-dogfood:qortex_observer:9cea5fef`, `design-pass-dogfood:qortex_observer:a090c803`, `design-pass-dogfood:qortex_observer:a09a384e`, `design-pass-dogfood:qortex_observer:a168c23d`, `design-pass-dogfood:qortex_observer:a27f3212`, `design-pass-dogfood:qortex_observer:a2963f43`, `design-pass-dogfood:qortex_observer:a7057853`, `design-pass-dogfood:qortex_observer:a70b45d7`, `design-pass-dogfood:qortex_observer:a7831e52`, `design-pass-dogfood:qortex_observer:acd19207`, `design-pass-dogfood:qortex_observer:ad55572c`, `design-pass-dogfood:qortex_observer:aeecbb13`, `design-pass-dogfood:qortex_observer:b33f203d`, `design-pass-dogfood:qortex_observer:b3719375`, `design-pass-dogfood:qortex_observer:b41546f0`, `design-pass-dogfood:qortex_observer:b64ee8c5`, `design-pass-dogfood:qortex_observer:b6974baf`, `design-pass-dogfood:qortex_observer:b791f0e4`, `design-pass-dogfood:qortex_observer:b7ea108c`, `design-pass-dogfood:qortex_observer:b8a35b68`, `design-pass-dogfood:qortex_observer:ba12377b`, `design-pass-dogfood:qortex_observer:bc1ed82a`, `design-pass-dogfood:qortex_observer:bd25541d`, `design-pass-dogfood:qortex_observer:bfacd8f2`, `design-pass-dogfood:qortex_observer:c00364fb`, `design-pass-dogfood:qortex_observer:c33a134a`, `design-pass-dogfood:qortex_observer:c4508df9`, `design-pass-dogfood:qortex_observer:c472e9de`, `design-pass-dogfood:qortex_observer:c58e848d`, `design-pass-dogfood:qortex_observer:c71db8cf`, `design-pass-dogfood:qortex_observer:c7a3f40b`, `design-pass-dogfood:qortex_observer:ccacaac7`, `design-pass-dogfood:qortex_observer:cce7f57f`, `design-pass-dogfood:qortex_observer:d04277bb`, `design-pass-dogfood:qortex_observer:d16ddc1a`, `design-pass-dogfood:qortex_observer:d4ec3d47`, `design-pass-dogfood:qortex_observer:d641d620`, `design-pass-dogfood:qortex_observer:d9ac86a0`, `design-pass-dogfood:qortex_observer:db9b4f80`, `design-pass-dogfood:qortex_observer:dca6f43e`, `design-pass-dogfood:qortex_observer:de7f125f`, `design-pass-dogfood:qortex_observer:e0efc527`, `design-pass-dogfood:qortex_observer:e1c642cc`, `design-pass-dogfood:qortex_observer:e2ea8829`, `design-pass-dogfood:qortex_observer:e344e802`, `design-pass-dogfood:qortex_observer:e84d8a73`, `design-pass-dogfood:qortex_observer:ea45fa14`, `design-pass-dogfood:qortex_observer:ea8a6567`, `design-pass-dogfood:qortex_observer:eaa94cdd`, `design-pass-dogfood:qortex_observer:ec063658`, `design-pass-dogfood:qortex_observer:ed0c2afb`, `design-pass-dogfood:qortex_observer:f210e7bb`, `design-pass-dogfood:qortex_observer:f29d590a`, `design-pass-dogfood:qortex_observer:f54cbe55`, `design-pass-dogfood:qortex_observer:f6488555`, `design-pass-dogfood:qortex_observer:f74947ec`, `design-pass-dogfood:qortex_observer:f9f7c90a`, `design-pass-dogfood:qortex_observer:fd4d1087`, `design-pass-dogfood:qortex_observer:fd9ca42b`, `design-pass-dogfood:security_karen:031981ce`, `design-pass-dogfood:security_karen:131cd1fa`, `design-pass-dogfood:security_karen:1af5a81f`, `design-pass-dogfood:security_karen:1cef48d5`, `design-pass-dogfood:security_karen:2896ba0c`, `design-pass-dogfood:security_karen:2bd9ec09`, `design-pass-dogfood:security_karen:3309a549`, `design-pass-dogfood:security_karen:35d8a3ad`, `design-pass-dogfood:security_karen:5f3ca636`, `design-pass-dogfood:security_karen:8caefcda`, `design-pass-dogfood:security_karen:a3bb96fb`, `design-pass-dogfood:security_karen:d05a9ea0`, `design-pass-dogfood:security_karen:fc20c8cd`, `design-pass-dogfood:test_persona:c17a1a6f`, `design-pass-dogfood:test_terrorist:0405a43f`, `design-pass-dogfood:test_terrorist:09a7159c`, `design-pass-dogfood:test_terrorist:1d2bd4c0`, `design-pass-dogfood:test_terrorist:1de18f9c`, `design-pass-dogfood:test_terrorist:206021f2`, `design-pass-dogfood:test_terrorist:2c42a90c`, `design-pass-dogfood:test_terrorist:2fb2bc96`, `design-pass-dogfood:test_terrorist:3016b866`, `design-pass-dogfood:test_terrorist:395ff740`, `design-pass-dogfood:test_terrorist:4790f26a`, `design-pass-dogfood:test_terrorist:81a6f504`, `design-pass-dogfood:test_terrorist:946b7a06`, `design-pass-dogfood:test_terrorist:9f320811`, `design-pass-dogfood:test_terrorist:a3d7df0e`, `design-pass-dogfood:test_terrorist:b7868f66`, `design-pass-dogfood:test_terrorist:bdd2bcbd`, `design-pass-dogfood:test_terrorist:c1ee7155`, `design-pass-dogfood:test_terrorist:eb977154`, `design-pass-dogfood:test_terrorist:efbb38b3`, `design-pass-dogfood:test_terrorist:f1378eae`, `design-pass-dogfood:test_terrorist:fe8aa547`, `general:bragi:02959dda`, `general:bragi:41e6758f`, `general:bragi:463490f1`, `general:bragi:63cd491d`, `general:bragi:92b40f51`, `general:bragi:9ecf1649`, `general:bragi:ce0516df`, `general:bragi:e1b5e53a`, `general:bragi:e30cf4d5`, `general:bragi:e4d6eee0`, `general:bragi:e96e9da4`, `general:bragi:eddf641e`, `general:bragi:f0384f94`, `general:qortex_design_patterns:009a418c`, `general:qortex_design_patterns:00c3a9ae`, `general:qortex_design_patterns:09ae8ef6`, `general:qortex_design_patterns:17b9abd6`, `general:qortex_design_patterns:1bbbbcd6`, `general:qortex_design_patterns:1c48dc87`, `general:qortex_design_patterns:20f54d8e`, `general:qortex_design_patterns:211f6637`, `general:qortex_design_patterns:316b0d61`, `general:qortex_design_patterns:48c2bf9e`, `general:qortex_design_patterns:52456083`, `general:qortex_design_patterns:539713dc`, `general:qortex_design_patterns:5421537f`, `general:qortex_design_patterns:604af93a`, `general:qortex_design_patterns:61562ff8`, `general:qortex_design_patterns:66aa7593`, `general:qortex_design_patterns:6895e4e3`, `general:qortex_design_patterns:77939dc5`, `general:qortex_design_patterns:7c32f1ce`, `general:qortex_design_patterns:95639de9`, `general:qortex_design_patterns:99cb1875`, `general:qortex_design_patterns:9e4ebfc8`, `general:qortex_design_patterns:af6223e4`, `general:qortex_design_patterns:bf779406`, `general:qortex_design_patterns:c07590d6`, `general:qortex_design_patterns:c1fd948b`, `general:qortex_design_patterns:c9566580`, `general:qortex_design_patterns:d1c0e664`, `general:qortex_design_patterns:d27ed782`, `general:qortex_design_patterns:d4f6ab83`, `general:qortex_design_patterns:ec1ddd87`, `general:qortex_design_patterns:f0aa24dd`, `general:qortex_impl_hiding:009a418c`, `general:qortex_impl_hiding:0ae292fe`, `general:qortex_impl_hiding:103c3fbf`, `general:qortex_impl_hiding:105cfe9f`, `general:qortex_impl_hiding:17b9abd6`, `general:qortex_impl_hiding:1bbbbcd6`, `general:qortex_impl_hiding:29c3da41`, `general:qortex_impl_hiding:2bf055c6`, `general:qortex_impl_hiding:2e300d8d`, `general:qortex_impl_hiding:42e88397`, `general:qortex_impl_hiding:44f68a58`, `general:qortex_impl_hiding:48c2bf9e`, `general:qortex_impl_hiding:4a9e5b86`, `general:qortex_impl_hiding:5ec0adfa`, `general:qortex_impl_hiding:6a975854`, `general:qortex_impl_hiding:6f219f32`, `general:qortex_impl_hiding:7974e474`, `general:qortex_impl_hiding:7c955b5d`, `general:qortex_impl_hiding:7dbbedeb`, `general:qortex_impl_hiding:9403a5c5`, `general:qortex_impl_hiding:9dafbed4`, `general:qortex_impl_hiding:9df38844`, `general:qortex_impl_hiding:a3edcfcd`, `general:qortex_impl_hiding:af6223e4`, `general:qortex_impl_hiding:c9566580`, `general:qortex_impl_hiding:c9c16146`, `general:qortex_impl_hiding:ca77185f`, `general:qortex_impl_hiding:d27ed782`, `general:qortex_impl_hiding:ded0c774`, `general:qortex_impl_hiding:e20d0f47`, `general:qortex_observer:04545c6d`, `general:qortex_observer:06600c9c`, `general:qortex_observer:09dfcf92`, `general:qortex_observer:0c77a1d2`, `general:qortex_observer:0c8ba2f8`, `general:qortex_observer:11fa7b98`, `general:qortex_observer:174f5a1b`, `general:qortex_observer:19c437b4`, `general:qortex_observer:19d8ee77`, `general:qortex_observer:1dc72bd4`, `general:qortex_observer:24343eba`, `general:qortex_observer:2743739e`, `general:qortex_observer:28f8af35`, `general:qortex_observer:2950435f`, `general:qortex_observer:2bc32ffe`, `general:qortex_observer:2dc10cf4`, `general:qortex_observer:2ec2b1ae`, `general:qortex_observer:2eebc331`, `general:qortex_observer:31d84cb5`, `general:qortex_observer:36576d68`, `general:qortex_observer:371ae9d9`, `general:qortex_observer:38723ff3`, `general:qortex_observer:39e88ecd`, `general:qortex_observer:3a4fec06`, `general:qortex_observer:3fed269e`, `general:qortex_observer:40645184`, `general:qortex_observer:434d864a`, `general:qortex_observer:436378ee`, `general:qortex_observer:44130d7d`, `general:qortex_observer:4875e202`, `general:qortex_observer:48a4251a`, `general:qortex_observer:4a0879c4`, `general:qortex_observer:4c1c9f4b`, `general:qortex_observer:4e97c534`, `general:qortex_observer:4e9e5c68`, `general:qortex_observer:4ff04b79`, `general:qortex_observer:507dc23d`, `general:qortex_observer:51783efd`, `general:qortex_observer:54ed2186`, `general:qortex_observer:59b7e51a`, `general:qortex_observer:59deb741`, `general:qortex_observer:5b87c2a4`, `general:qortex_observer:5bd02b0d`, `general:qortex_observer:5cd6a960`, `general:qortex_observer:5d7d7338`, `general:qortex_observer:5d85527d`, `general:qortex_observer:5df8aa0a`, `general:qortex_observer:5f14d5c7`, `general:qortex_observer:610bcbcf`, `general:qortex_observer:61865b50`, `general:qortex_observer:64c6645d`, `general:qortex_observer:685b1c2e`, `general:qortex_observer:6a17181c`, `general:qortex_observer:6d945803`, `general:qortex_observer:7180acf9`, `general:qortex_observer:72bfc672`, `general:qortex_observer:76c35779`, `general:qortex_observer:78868c08`, `general:qortex_observer:78d156fc`, `general:qortex_observer:7930bb43`, `general:qortex_observer:7b88ef97`, `general:qortex_observer:7dd1e3da`, `general:qortex_observer:821a35c2`, `general:qortex_observer:856a93fc`, `general:qortex_observer:882600a9`, `general:qortex_observer:884e7f55`, `general:qortex_observer:888dd5da`, `general:qortex_observer:8fe1f256`, `general:qortex_observer:9042bdf1`, `general:qortex_observer:90765dde`, `general:qortex_observer:93dcbbb5`, `general:qortex_observer:961a4140`, `general:qortex_observer:964c28c2`, `general:qortex_observer:98b12a07`, `general:qortex_observer:9acc67a4`, `general:qortex_observer:9c164033`, `general:qortex_observer:9cea5fef`, `general:qortex_observer:a090c803`, `general:qortex_observer:a09a384e`, `general:qortex_observer:a168c23d`, `general:qortex_observer:a27f3212`, `general:qortex_observer:a2963f43`, `general:qortex_observer:a7057853`, `general:qortex_observer:a70b45d7`, `general:qortex_observer:a7831e52`, `general:qortex_observer:acd19207`, `general:qortex_observer:ad55572c`, `general:qortex_observer:aeecbb13`, `general:qortex_observer:b33f203d`, `general:qortex_observer:b3719375`, `general:qortex_observer:b41546f0`, `general:qortex_observer:b64ee8c5`, `general:qortex_observer:b6974baf`, `general:qortex_observer:b791f0e4`, `general:qortex_observer:b7ea108c`, `general:qortex_observer:b8a35b68`, `general:qortex_observer:ba12377b`, `general:qortex_observer:bc1ed82a`, `general:qortex_observer:bd25541d`, `general:qortex_observer:bfacd8f2`, `general:qortex_observer:c00364fb`, `general:qortex_observer:c33a134a`, `general:qortex_observer:c4508df9`, `general:qortex_observer:c472e9de`, `general:qortex_observer:c58e848d`, `general:qortex_observer:c71db8cf`, `general:qortex_observer:c7a3f40b`, `general:qortex_observer:ccacaac7`, `general:qortex_observer:cce7f57f`, `general:qortex_observer:d04277bb`, `general:qortex_observer:d16ddc1a`, `general:qortex_observer:d4ec3d47`, `general:qortex_observer:d641d620`, `general:qortex_observer:d9ac86a0`, `general:qortex_observer:db9b4f80`, `general:qortex_observer:dca6f43e`, `general:qortex_observer:de7f125f`, `general:qortex_observer:e0efc527`, `general:qortex_observer:e1c642cc`, `general:qortex_observer:e2ea8829`, `general:qortex_observer:e344e802`, `general:qortex_observer:e84d8a73`, `general:qortex_observer:ea45fa14`, `general:qortex_observer:ea8a6567`, `general:qortex_observer:eaa94cdd`, `general:qortex_observer:ec063658`, `general:qortex_observer:ed0c2afb`, `general:qortex_observer:f210e7bb`, `general:qortex_observer:f29d590a`, `general:qortex_observer:f54cbe55`, `general:qortex_observer:f6488555`, `general:qortex_observer:f74947ec`, `general:qortex_observer:f9f7c90a`, `general:qortex_observer:fd4d1087`, `general:qortex_observer:fd9ca42b`, `general:security_karen:031981ce`, `general:security_karen:131cd1fa`, `general:security_karen:1af5a81f`, `general:security_karen:1cef48d5`, `general:security_karen:2896ba0c`, `general:security_karen:2bd9ec09`, `general:security_karen:3309a549`, `general:security_karen:35d8a3ad`, `general:security_karen:5f3ca636`, `general:security_karen:8caefcda`, `general:security_karen:a3bb96fb`, `general:security_karen:d05a9ea0`, `general:security_karen:fc20c8cd`, `general:test_persona:c17a1a6f`, `general:test_terrorist:0405a43f`, `general:test_terrorist:09a7159c`, `general:test_terrorist:1d2bd4c0`, `general:test_terrorist:1de18f9c`, `general:test_terrorist:206021f2`, `general:test_terrorist:2c42a90c`, `general:test_terrorist:2fb2bc96`, `general:test_terrorist:3016b866`, `general:test_terrorist:395ff740`, `general:test_terrorist:4790f26a`, `general:test_terrorist:81a6f504`, `general:test_terrorist:946b7a06`, `general:test_terrorist:9f320811`, `general:test_terrorist:a3d7df0e`, `general:test_terrorist:b7868f66`, `general:test_terrorist:bdd2bcbd`, `general:test_terrorist:c1ee7155`, `general:test_terrorist:eb977154`, `general:test_terrorist:efbb38b3`, `general:test_terrorist:f1378eae`, `general:test_terrorist:fe8aa547` (mean ≥ 0.7).

Session ran for 41.8 minutes. with 252 rules at start → 252 at end.

> Every session that measures improvement is evidence that the learning loop works. buildlog tracks what changed, learns which rules help, and improves automatically.

### Metrics

| Metric | This Session | Prior | Trend |
|--------|-------------|-------|-------|
| Mistakes caught | 0 | — | — |
| Repeated mistakes | 0 | — | — |
| Rules at start | 252 | — | — |
| Rules at end | 252 | — | — |
| Mean reward | — | — | — |

### Top Rules

| Rule | Mean | Observations | Status |
|------|------|-------------|--------|
| `design-pass-dogfood:bragi:02959dda` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:2e966d45` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:41e6758f` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:463490f1` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:63cd491d` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:71344189` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:92b40f51` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:980ac190` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:9ecf1649` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:c33015a1` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:c651da02` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:ce0516df` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:d14b1a2d` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:d720e228` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:e1b5e53a` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:e30cf4d5` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:e4d6eee0` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:e96e9da4` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:eddf641e` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:bragi:f0384f94` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:4a9e5b86` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:031981ce` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:131cd1fa` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:1af5a81f` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:1cef48d5` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:2896ba0c` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:2bd9ec09` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:3309a549` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:35d8a3ad` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:5f3ca636` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:8caefcda` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:a3bb96fb` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:d05a9ea0` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:security_karen:fc20c8cd` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_persona:c17a1a6f` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:0405a43f` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:09a7159c` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:1d2bd4c0` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:1de18f9c` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:206021f2` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:2c42a90c` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:2fb2bc96` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:3016b866` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:395ff740` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:4790f26a` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:81a6f504` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:946b7a06` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:9f320811` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:a3d7df0e` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:b7868f66` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:bdd2bcbd` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:c1ee7155` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:eb977154` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:efbb38b3` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:f1378eae` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:test_terrorist:fe8aa547` | 0.7500 | 2 | earned confidence |
| `general:bragi:02959dda` | 0.7500 | 2 | earned confidence |
| `general:bragi:41e6758f` | 0.7500 | 2 | earned confidence |
| `general:bragi:463490f1` | 0.7500 | 2 | earned confidence |
| `general:bragi:63cd491d` | 0.7500 | 2 | earned confidence |
| `general:bragi:92b40f51` | 0.7500 | 2 | earned confidence |
| `general:bragi:9ecf1649` | 0.7500 | 2 | earned confidence |
| `general:bragi:ce0516df` | 0.7500 | 2 | earned confidence |
| `general:bragi:e1b5e53a` | 0.7500 | 2 | earned confidence |
| `general:bragi:e30cf4d5` | 0.7500 | 2 | earned confidence |
| `general:bragi:e4d6eee0` | 0.7500 | 2 | earned confidence |
| `general:bragi:e96e9da4` | 0.7500 | 2 | earned confidence |
| `general:bragi:eddf641e` | 0.7500 | 2 | earned confidence |
| `general:bragi:f0384f94` | 0.7500 | 2 | earned confidence |
| `general:qortex_impl_hiding:4a9e5b86` | 0.7500 | 2 | earned confidence |
| `general:security_karen:031981ce` | 0.7500 | 2 | earned confidence |
| `general:security_karen:131cd1fa` | 0.7500 | 2 | earned confidence |
| `general:security_karen:1af5a81f` | 0.7500 | 2 | earned confidence |
| `general:security_karen:1cef48d5` | 0.7500 | 2 | earned confidence |
| `general:security_karen:2896ba0c` | 0.7500 | 2 | earned confidence |
| `general:security_karen:2bd9ec09` | 0.7500 | 2 | earned confidence |
| `general:security_karen:3309a549` | 0.7500 | 2 | earned confidence |
| `general:security_karen:35d8a3ad` | 0.7500 | 2 | earned confidence |
| `general:security_karen:5f3ca636` | 0.7500 | 2 | earned confidence |
| `general:security_karen:8caefcda` | 0.7500 | 2 | earned confidence |
| `general:security_karen:a3bb96fb` | 0.7500 | 2 | earned confidence |
| `general:security_karen:d05a9ea0` | 0.7500 | 2 | earned confidence |
| `general:security_karen:fc20c8cd` | 0.7500 | 2 | earned confidence |
| `general:test_persona:c17a1a6f` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:0405a43f` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:09a7159c` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:1d2bd4c0` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:1de18f9c` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:206021f2` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:2c42a90c` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:2fb2bc96` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:3016b866` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:395ff740` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:4790f26a` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:81a6f504` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:946b7a06` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:9f320811` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:a3d7df0e` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:b7868f66` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:bdd2bcbd` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:c1ee7155` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:eb977154` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:efbb38b3` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:f1378eae` | 0.7500 | 2 | earned confidence |
| `general:test_terrorist:fe8aa547` | 0.7500 | 2 | earned confidence |
| `design-pass-dogfood:qortex_observer:1dc72bd4` | 0.7475 | 1 | earned confidence |
| `general:qortex_observer:1dc72bd4` | 0.7475 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b33f203d` | 0.7449 | 1 | earned confidence |
| `general:qortex_observer:b33f203d` | 0.7449 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:48c2bf9e` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:04545c6d` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:06600c9c` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:19c437b4` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:78d156fc` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b64ee8c5` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f6488555` | 0.7436 | 1 | earned confidence |
| `general:qortex_impl_hiding:48c2bf9e` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:04545c6d` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:06600c9c` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:19c437b4` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:78d156fc` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:b64ee8c5` | 0.7436 | 1 | earned confidence |
| `general:qortex_observer:f6488555` | 0.7436 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:7180acf9` | 0.7423 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:e1c642cc` | 0.7423 | 1 | earned confidence |
| `general:qortex_observer:7180acf9` | 0.7423 | 1 | earned confidence |
| `general:qortex_observer:e1c642cc` | 0.7423 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:7930bb43` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:884e7f55` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:8fe1f256` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:9acc67a4` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c4508df9` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c7a3f40b` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:7930bb43` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:884e7f55` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:8fe1f256` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:9acc67a4` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:c4508df9` | 0.7409 | 1 | earned confidence |
| `general:qortex_observer:c7a3f40b` | 0.7409 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2743739e` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:36576d68` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:48a4251a` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5bd02b0d` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:61865b50` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:685b1c2e` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c71db8cf` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:db9b4f80` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ed0c2afb` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f210e7bb` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:2743739e` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:36576d68` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:48a4251a` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:5bd02b0d` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:61865b50` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:685b1c2e` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:c71db8cf` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:db9b4f80` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:ed0c2afb` | 0.7396 | 1 | earned confidence |
| `general:qortex_observer:f210e7bb` | 0.7396 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:09dfcf92` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:28f8af35` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:40645184` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5d7d7338` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:7b88ef97` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:90765dde` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ba12377b` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:09dfcf92` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:28f8af35` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:40645184` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:5d7d7338` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:7b88ef97` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:90765dde` | 0.7382 | 1 | earned confidence |
| `general:qortex_observer:ba12377b` | 0.7382 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:009a418c` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:00c3a9ae` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:09ae8ef6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:17b9abd6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:1bbbbcd6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:1c48dc87` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:20f54d8e` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:211f6637` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:316b0d61` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:48c2bf9e` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:52456083` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:539713dc` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:5421537f` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:604af93a` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:61562ff8` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:66aa7593` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:6895e4e3` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:77939dc5` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:7c32f1ce` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:95639de9` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:99cb1875` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:9e4ebfc8` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:af6223e4` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:bf779406` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:c07590d6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:c1fd948b` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:c9566580` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:d1c0e664` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:d27ed782` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:d4f6ab83` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:ec1ddd87` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_design_patterns:f0aa24dd` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:0ae292fe` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:103c3fbf` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:105cfe9f` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:17b9abd6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:1bbbbcd6` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:29c3da41` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:42e88397` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:44f68a58` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:6a975854` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:7974e474` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:7dbbedeb` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:9403a5c5` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:a3edcfcd` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:c9566580` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:ca77185f` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:d27ed782` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:ded0c774` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:e20d0f47` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:0c8ba2f8` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:19d8ee77` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2ec2b1ae` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:31d84cb5` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5cd6a960` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:78868c08` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:7dd1e3da` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a27f3212` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b3719375` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:bc1ed82a` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c00364fb` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c472e9de` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ccacaac7` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:d04277bb` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:e84d8a73` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ea8a6567` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:eaa94cdd` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f9f7c90a` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:fd4d1087` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:009a418c` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:00c3a9ae` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:09ae8ef6` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:17b9abd6` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:1bbbbcd6` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:1c48dc87` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:20f54d8e` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:211f6637` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:316b0d61` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:48c2bf9e` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:52456083` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:539713dc` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:5421537f` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:604af93a` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:61562ff8` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:66aa7593` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:6895e4e3` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:77939dc5` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:7c32f1ce` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:95639de9` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:99cb1875` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:9e4ebfc8` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:af6223e4` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:bf779406` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:c07590d6` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:c1fd948b` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:c9566580` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:d1c0e664` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:d27ed782` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:d4f6ab83` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:ec1ddd87` | 0.7368 | 1 | earned confidence |
| `general:qortex_design_patterns:f0aa24dd` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:0ae292fe` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:103c3fbf` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:105cfe9f` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:17b9abd6` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:1bbbbcd6` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:29c3da41` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:42e88397` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:44f68a58` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:6a975854` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:7974e474` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:7dbbedeb` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:9403a5c5` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:a3edcfcd` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:c9566580` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:ca77185f` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:d27ed782` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:ded0c774` | 0.7368 | 1 | earned confidence |
| `general:qortex_impl_hiding:e20d0f47` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:0c8ba2f8` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:19d8ee77` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:2ec2b1ae` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:31d84cb5` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:5cd6a960` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:78868c08` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:7dd1e3da` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:a27f3212` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:b3719375` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:bc1ed82a` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:c00364fb` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:c472e9de` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:ccacaac7` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:d04277bb` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:e84d8a73` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:ea8a6567` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:eaa94cdd` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:f9f7c90a` | 0.7368 | 1 | earned confidence |
| `general:qortex_observer:fd4d1087` | 0.7368 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:0c77a1d2` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:11fa7b98` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2dc10cf4` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:38723ff3` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:3a4fec06` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4ff04b79` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:6a17181c` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b7ea108c` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b8a35b68` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:d641d620` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:0c77a1d2` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:11fa7b98` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:2dc10cf4` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:38723ff3` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:3a4fec06` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:4ff04b79` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:6a17181c` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:b7ea108c` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:b8a35b68` | 0.7354 | 1 | earned confidence |
| `general:qortex_observer:d641d620` | 0.7354 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:174f5a1b` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:24343eba` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2950435f` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4a0879c4` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4c1c9f4b` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5b87c2a4` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5f14d5c7` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:882600a9` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:9042bdf1` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:964c28c2` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:98b12a07` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a2963f43` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a7831e52` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b791f0e4` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:cce7f57f` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:d4ec3d47` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:d9ac86a0` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:dca6f43e` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:e0efc527` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ec063658` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f29d590a` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f54cbe55` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:f74947ec` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:174f5a1b` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:24343eba` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:2950435f` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:4a0879c4` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:4c1c9f4b` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:5b87c2a4` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:5f14d5c7` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:882600a9` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:9042bdf1` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:964c28c2` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:98b12a07` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:a2963f43` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:a7831e52` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:b791f0e4` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:cce7f57f` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:d4ec3d47` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:d9ac86a0` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:dca6f43e` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:e0efc527` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:ec063658` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:f29d590a` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:f54cbe55` | 0.7340 | 1 | earned confidence |
| `general:qortex_observer:f74947ec` | 0.7340 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:371ae9d9` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:436378ee` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:44130d7d` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4875e202` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:59deb741` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:64c6645d` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:821a35c2` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:856a93fc` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:9c164033` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a168c23d` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a7057853` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:acd19207` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:aeecbb13` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c58e848d` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:d16ddc1a` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:fd9ca42b` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:371ae9d9` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:436378ee` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:44130d7d` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:4875e202` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:59deb741` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:64c6645d` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:821a35c2` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:856a93fc` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:9c164033` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:a168c23d` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:a7057853` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:acd19207` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:aeecbb13` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:c58e848d` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:d16ddc1a` | 0.7326 | 1 | earned confidence |
| `general:qortex_observer:fd9ca42b` | 0.7326 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4e97c534` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:4e9e5c68` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:6d945803` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:72bfc672` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:961a4140` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a090c803` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a09a384e` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:bd25541d` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:4e97c534` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:4e9e5c68` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:6d945803` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:72bfc672` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:961a4140` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:a090c803` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:a09a384e` | 0.7312 | 1 | earned confidence |
| `general:qortex_observer:bd25541d` | 0.7312 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:009a418c` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:af6223e4` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2eebc331` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:39e88ecd` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:3fed269e` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:434d864a` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:507dc23d` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:51783efd` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:59b7e51a` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5d85527d` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:5df8aa0a` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:610bcbcf` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:76c35779` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:93dcbbb5` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:9cea5fef` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:a70b45d7` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ad55572c` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b41546f0` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:bfacd8f2` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:c33a134a` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:de7f125f` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:e2ea8829` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:e344e802` | 0.7297 | 1 | earned confidence |
| `general:qortex_impl_hiding:009a418c` | 0.7297 | 1 | earned confidence |
| `general:qortex_impl_hiding:af6223e4` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:2eebc331` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:39e88ecd` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:3fed269e` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:434d864a` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:507dc23d` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:51783efd` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:59b7e51a` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:5d85527d` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:5df8aa0a` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:610bcbcf` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:76c35779` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:93dcbbb5` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:9cea5fef` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:a70b45d7` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:ad55572c` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:b41546f0` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:bfacd8f2` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:c33a134a` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:de7f125f` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:e2ea8829` | 0.7297 | 1 | earned confidence |
| `general:qortex_observer:e344e802` | 0.7297 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:2bc32ffe` | 0.7283 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:888dd5da` | 0.7283 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:ea45fa14` | 0.7283 | 1 | earned confidence |
| `general:qortex_observer:2bc32ffe` | 0.7283 | 1 | earned confidence |
| `general:qortex_observer:888dd5da` | 0.7283 | 1 | earned confidence |
| `general:qortex_observer:ea45fa14` | 0.7283 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:b6974baf` | 0.7268 | 1 | earned confidence |
| `general:qortex_observer:b6974baf` | 0.7268 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:2bf055c6` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:2e300d8d` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:5ec0adfa` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:6f219f32` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:7c955b5d` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:9dafbed4` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:9df38844` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_observer:54ed2186` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:2bf055c6` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:2e300d8d` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:5ec0adfa` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:6f219f32` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:7c955b5d` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:9dafbed4` | 0.7222 | 1 | earned confidence |
| `general:qortex_impl_hiding:9df38844` | 0.7222 | 1 | earned confidence |
| `general:qortex_observer:54ed2186` | 0.7222 | 1 | earned confidence |
| `design-pass-dogfood:qortex_impl_hiding:c9c16146` | 0.7059 | 1 | earned confidence |
| `general:qortex_impl_hiding:c9c16146` | 0.7059 | 1 | earned confidence |
| `general:bragi:2e966d45` | 0.6667 | 1 | stable |
| `general:bragi:71344189` | 0.6667 | 1 | stable |
| `general:bragi:980ac190` | 0.6667 | 1 | stable |
| `general:bragi:c33015a1` | 0.6667 | 1 | stable |
| `general:bragi:c651da02` | 0.6667 | 1 | stable |
| `general:bragi:d14b1a2d` | 0.6667 | 1 | stable |
| `general:bragi:d720e228` | 0.6667 | 1 | stable |

<!-- buildlog:session-summary:session-20260307-010145-825714 -->
<!-- buildlog:improvements:end -->

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


### `731882e` — chore: buildlog entry update for Phase 5 + PR #2

Files:
- `buildlog/2026-03-06-hunter-ejection.md`


### `3e4f8c8` — feat: Phase 6 — SVG pipeline diagram, CI/CD eval gate, v1.0.0 prep

Files:
- `.github/workflows/eval-gate.yml`
- `README.md`
- `public/pipeline-diagram.svg`

