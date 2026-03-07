<div align="center">

# 🎯 Offer Scope

### Persona insights to shippable 1-day build spec

[![Pipeline Stage](https://img.shields.io/badge/Stage-4_of_6-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()

**Scope what ships. In 8 hours or less.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Offer Scope transforms persona extraction output and signal scan SPEND data into a complete 1-day build spec. It selects the highest-value decision point from the persona data, applies Hormozi's value equation, selects a format constrained by ship time and consumption context, generates positioning copy using the persona's exact language, plans distribution, and models revenue with conservative assumptions.

The output is everything needed to sit down and build: sections with time estimates, positioning copy, distribution plan, revenue projections, and kill criteria -- all before writing a single line of product content.

## Pipeline Position

```
signal-scan ───▶ decision-log ───▶ persona-extract ───▶ ╔══════════════╗
                                                        ║              ║
                                                        ║  offer-scope ║───▶ pitch ───▶ hunter-log
                                                        ║  ** here **  ║
                                                        ║              ║
                                                        ╚══════════════╝
```

## Input

| Field | Source | Required |
|---|---|---|
| **Persona** | Pain stories, decision triggers, objections, WTP, channels | Yes |
| **SPEND signals** | Price ranges, volume evidence, platforms from signal scan | Yes |
| **Domain + opportunity** | The specific domain and opportunity being targeted | Yes |
| **Constraints** | Ship time (default: 1 day), format preferences | Optional |

## Output

Two files written to `Admin/Product-Discovery/Offers/`:

| File | Format | Contents |
|---|---|---|
| `offer-scope-{domain}-{date}.json` | JSON | Structured build spec validated against schema |
| `{domain}-{date}.md` | Markdown | Decision point, value equation, build spec, positioning, distribution, revenue model |

## Workflow

```
Persona + SPEND Data + Constraints
    │
    ├── Phase 1: Decision Point Selection ─── highest pain * urgency * WTP composite
    │
    ├── Phase 2: Value Equation (Hormozi) ─── dream outcome, likelihood, time delay, effort
    │
    ├── Phase 3: Format Selection ─── ship time × consumption context × price ceiling
    │
    ├── Phase 4: Scope Definition ─── 4-7 sections, total ≤ 8 hours, one transformation
    │
    ├── Phase 5: Positioning & Copy ─── headline, bullets, objection handlers, guarantee
    │
    ├── Phase 6: Distribution Planning ─── specific channel + value-first launch strategy
    │
    └── Phase 7: Revenue Modeling ─── conservative projections + kill criteria
```

### Value Equation (Hormozi)

| Quadrant | Maximize/Minimize | Source |
|---|---|---|
| **Dream Outcome** | Maximize | Persona's emotional state + desired future |
| **Perceived Likelihood** | Maximize | Operator's real credentials + social proof |
| **Time Delay** | Minimize | Fastest possible win upon purchase |
| **Effort & Sacrifice** | Minimize | What the product does FOR them |

### Format Selection Matrix

| Ship Time | Viable Formats |
|---|---|
| 1 day (6-8h) | PDF, Notion template, short video |
| 3 days | PDF bundle, video series, workshop outline |
| 1 week | Mini-course, tool/calculator, community launch |
| 2+ weeks | Full course, SaaS, comprehensive tool |

### Scope Rules

- Total build time must not exceed 8 hours
- Every section serves the single transformation
- At least one section must be a decision tool (decision tree, checklist, framework)
- "Quick Start" section first for immediate value

## Usage

```
"Scope an offer for [persona]"
"Build spec from this persona"
"What should I ship for [persona] in one day?"
"Turn this persona into a product"
"/offer-scope [persona-extract output]"
```

## Quality Checklist

- [ ] Decision point connects to 3+ persona pain stories
- [ ] Value equation uses persona language, not marketer-speak
- [ ] Format matches consumption context (2 AM ≠ video course)
- [ ] Total build time ≤ 8 hours with per-section estimates
- [ ] Headline uses persona's actual language
- [ ] Price anchored to SPEND data
- [ ] Distribution targets a specific community/platform
- [ ] Launch strategy follows value-first (give 80%, sell the packaged version)
- [ ] Kill criteria defined before building
- [ ] Revenue targets conservative with stated assumptions
- [ ] JSON validates against schema
- [ ] Pipeline kanban updated

---

<div align="center">

**Scope creep here kills the 1-day timeline. Cut ruthlessly.**

[![Pipeline](https://img.shields.io/badge/Prev-persona--extract-8b949e?style=for-the-badge)]()
[![Pipeline](https://img.shields.io/badge/Next-pitch-4DCFC9?style=for-the-badge)]()

</div>
