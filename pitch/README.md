<div align="center">

# 🚀 Pitch

### Go-to-market launch engine with deployable assets

[![Pipeline Stage](https://img.shields.io/badge/Stage-5_of_6-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()

**From validated spec to launch-ready in one session.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Pitch is the synthesis layer. It takes a validated offer spec and every upstream artifact (persona, SWOT, decision, signal scan) and produces the complete go-to-market launch kit: paste-ready landing page copy, platform-specific launch posts, a GitHub product README with product structure sketch, a 5-email post-purchase sequence, an hour-by-hour launch checklist, an A/B test spec, kill criteria refinement, and a Slidev presentation.

It also deploys: launchpad repo branch with PR, landing page HTML for Vercel preview, content seeds for LinWheel, launch kanban for Obsidian, and review reminders with Obsidian Tasks dates. Everything needed to go live.

## Pipeline Position

```
signal-scan ───▶ decision-log ───▶ persona-extract ───▶ offer-scope ───▶ ╔══════════╗
                                                                         ║          ║
                                                                         ║  pitch   ║───▶ hunter-log
                                                                         ║ ** here **║
                                                                         ║          ║
                                                                         ╚════╤═════╝
                                                                              │
                                                              ┌───────────────┼───────────────┐
                                                              ▼               ▼               ▼
                                                        landing-page    slidev-deck      one-pager
```

## Input

| Field | Source | Required |
|---|---|---|
| **Offer spec** | Product name, format, price, value equation, build spec, positioning, distribution, revenue model, kill criteria | Yes |
| **Persona** | Pain stories, triggers, objections, WTP, channels | Yes |
| **SWOT** | Verdict, validated strengths, key risks, modifications, moat | Yes |
| **Decision log** | Rationale, constraints, SDP scores | Yes |
| **Signal scan** | PAIN, SPEND, COMPETITIVE, AUDIENCE signals | Yes |

## Output

### Vault Files

| File | Location | Contents |
|---|---|---|
| Pitch JSON | `Pitches/{product}-pitch-{date}.json` | Structured launch kit |
| Pitch Markdown | `Pitches/{product}-pitch-{date}.md` | All phase outputs |
| Content Seeds | `Writing/Content-Briefs/{product}-{platform}-seed-{date}.md` | Launch post drafts |
| Launch Kanban | `Pitches/{product}-launch-checklist.kanban.md` | Dated execution tasks |
| Pipeline Tracker | `Pipeline.md` | Product row with branch/preview links |

### Deployment Artifacts

| Artifact | Location | Description |
|---|---|---|
| Product Branch | `github.com/Peleke/launchpad` branch `{product-slug}` | Structure, README, emails |
| Landing Page | `index.html` on branch root | Dark theme, mobile, zero deps |
| PR | Against `main` | Review checklist |
| Vercel Preview | `launchpad-git-{slug}-*.vercel.app` | Instant deploy |

## Workflow

```
Offer Spec + Persona + SWOT + Decision + Signal Scan
    │
    ├── Phase 1: Landing Page Copy ─── complete, paste-ready, every line traces to persona
    │
    ├── Phase 2: Launch Posts ─── 2-3 platform-specific, 80/20 value-first, content seeds
    │
    ├── Phase 3: GitHub Product Repo ─── structure sketch + README + hero + branch/PR + HTML
    │
    ├── Phase 4: Email Sequence ─── 5 emails: welcome, quick win, story, objection, community
    │
    ├── Phase 5: Launch Checklist ─── pre-launch through Month 2-3, Obsidian kanban output
    │
    ├── Phase 6: A/B Test Spec ─── headline/price variants, metrics, decision thresholds
    │
    ├── Phase 7: Kill Criteria ─── Week 1, Month 1, Month 3 with Obsidian Tasks dates
    │
    └── Phase 8: Slide Deck ─── Slidev with extracted SVGs and speaker notes
```

### Modes

| Mode | Description |
|---|---|
| **Full** | Generate all phases from upstream data |
| **Deploy-only** | Read existing pitch from vault, split and deploy artifacts |

Deploy-only reads an existing pitch markdown and runs only the deployment phases (content seeds, HTML, emails, kanban, review reminders) -- useful when re-deploying after edits.

### Content Pipeline Integration

LinkedIn launch posts get the `::linkedin` prefix (byte 0, line 1) which triggers: Obsidian note → OpenClaw → LinWheel → LinkedIn publication.

## Usage

```
"Build the launch kit for [product]"
"Create pitch materials from this offer spec"
"I need landing page copy for [product]"
"Generate the go-to-market package"
"/pitch [offer-scope output]"
"/pitch --deploy-only [pitch-slug]"
```

## Quality Checklist

- [ ] Landing page copy is complete and standalone
- [ ] Every copy line traces to persona data or offer-scope positioning
- [ ] Problem section uses 3+ persona pain story references
- [ ] Launch posts follow 80/20 value-first and platform norms
- [ ] GitHub README is documentation first, marketing second
- [ ] Email sequence: specific subjects, one job per email, practitioner tone
- [ ] Launch checklist: specific dates, times, platforms (hour-by-hour on launch day)
- [ ] Kill criteria are time-bounded and measurable
- [ ] Landing page HTML: mobile responsive, zero deps, OG tags
- [ ] PR opened against launchpad main
- [ ] Content seeds in `Writing/Content-Briefs/`
- [ ] Launch kanban with dated tasks
- [ ] Pipeline tracker updated
- [ ] All prose reviewed by Bragi persona

---

<div align="center">

**Everything needed to go live. Nothing left to figure out.**

[![Pipeline](https://img.shields.io/badge/Prev-offer--scope-8b949e?style=for-the-badge)]()
[![Pipeline](https://img.shields.io/badge/Next-hunter--log-4DCFC9?style=for-the-badge)]()

</div>
