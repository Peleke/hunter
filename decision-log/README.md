<div align="center">

# ⚖️ Decision Log

### Structured go/no-go decisions with evidence and kill criteria

[![Pipeline Stage](https://img.shields.io/badge/Stage-2_of_6-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()
[![Context](https://img.shields.io/badge/Context-Fork-facc15?style=for-the-badge)]()

**Choose what to build using data, not excitement.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Decision Log takes signal scan output and operator preferences, runs them through the Signal-to-Decision Pipeline (SDP), and produces a structured decision record. It re-scores opportunities across 6 weighted dimensions, applies strategic filters (Bezos two-way door, Helmer 7 Powers), runs a cognitive bias audit, executes a pre-mortem, and sets measurable kill criteria -- all before a single line of product code is written.

The output is a fully logged decision with alternatives rejected, confidence level, revisit date, and a pre-filled hint for persona-extract.

## Pipeline Position

```
signal-scan ───▶ ╔══════════════╗
                 ║              ║
                 ║ decision-log ║───▶ persona-extract ───▶ offer-scope ───▶ pitch ───▶ hunter-log
                 ║  ** here **  ║
                 ║              ║
                 ╚══════════════╝
```

## Input

| Field | Source | Required |
|---|---|---|
| **Signal scan output** | Completed signal-scan with all 7 types, opportunities, meta-signal | Yes |
| **Chosen opportunity** | Which opportunity to pursue (or guided selection) | Yes |
| **Rationale** | Why this one over others | Yes |
| **Constraints** | "Must ship in 1 day", "PDF only", "under $50" | Yes |
| **Alternatives rejected** | Each with title and rejection reason | Yes |

## Output

Two files written to `Admin/Product-Discovery/Decisions/`:

| File | Format | Contents |
|---|---|---|
| `{domain}-{date}.json` | JSON | Structured decision record validated against schema |
| `{domain}-{date}.md` | Markdown | Decision frame, SDP scores, rationale, bias check, pre-mortem, kill criteria |

## Workflow

```
Signal Scan Output + Operator Preferences
    │
    ├── Phase 1: Frame the Decision ─── what exactly are we deciding?
    │
    ├── Phase 2: Present Options ─── ranked from scan, including "do nothing"
    │
    ├── Phase 3: SDP Scoring ─── 6 dimensions, fresh scoring pass
    │
    ├── Phase 4: Strategic Filters ─── reversibility check + power assessment
    │
    ├── Phase 5: Bias Check ─── confirmation, availability, anchoring, excitement
    │
    ├── Phase 6: Pre-Mortem ─── 3+ failure scenarios (execution, market, competition)
    │
    ├── Phase 7: Make the Call ─── record chosen + rejected + rationale
    │
    ├── Phase 8: Kill Criteria ─── measurable, time-bounded conditions to abandon
    │
    └── Phase 9: Next-Step Hint ─── pre-fill persona-extract inputs
```

### SDP Scoring Framework

| Dimension | Scale | Weight | Measures |
|---|---|---|---|
| Pain Intensity | 1-5 | 3x | Vitamin (1-2) or painkiller (4-5)? |
| Spend Evidence | 1-5 | 3x | Are people already paying for worse? |
| Edge Match | 1-5 | 2x | Does this play to the operator's strengths? |
| Time to Ship | 1-5 | 2x | How fast can a testable version ship? |
| Competition Gap | 1-5 | 1x | Is there a meaningful opening? |
| Audience Fit | 1-5 | 1x | Does the operator have access to these people? |

**SDP Score** = `(pain*3) + (spend*3) + (edge*2) + (time*2) + (gap) + (audience)` — Range: 12 to 60.

### Strategic Filters

| Filter | Framework | Question |
|---|---|---|
| **Reversibility** | Bezos Two-Way Door | If this fails, can you undo it in 2 weeks? |
| **Power** | Helmer 7 Powers | Does success create counter-positioning, switching costs, or cornered resource? |

### Bias Audit

Four explicit checks: confirmation bias (data vs. wanting), availability bias (top-of-mind vs. best), anchoring (original rank vs. SDP rank), excitement bias (would you tell a friend to do this?).

## Usage

```
"Which opportunity should I pursue?"
"Help me decide what to build from this scan"
"Run a decision log on [signal scan output]"
"Log a decision for [opportunity]"
"/decision-log [signal scan output]"
```

## Quality Checklist

- [ ] Decision frame states what is being decided, time horizon, success metric
- [ ] All opportunities presented with SDP scores, not just the chosen one
- [ ] Rationale references specific signal data (quotes, numbers, sources)
- [ ] Every alternative has a specific rejection reason
- [ ] Pre-mortem has 3+ failure scenarios (execution, market, competition)
- [ ] Kill criteria are measurable and time-bounded
- [ ] Next-step hint populated for persona-extract
- [ ] JSON validates against schema
- [ ] Pipeline kanban updated

---

<div align="center">

**Record the decision before emotional investment clouds judgment.**

[![Pipeline](https://img.shields.io/badge/Prev-signal--scan-8b949e?style=for-the-badge)]()
[![Pipeline](https://img.shields.io/badge/Next-persona--extract-4DCFC9?style=for-the-badge)]()

</div>
