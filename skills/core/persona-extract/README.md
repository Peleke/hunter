<div align="center">

# 👤 Persona Extract

### Evidence-based buyer personas from real community stories

[![Pipeline Stage](https://img.shields.io/badge/Stage-3_of_6-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()
[![Context](https://img.shields.io/badge/Context-Fork-facc15?style=for-the-badge)]()

**Real people, real quotes, real pain. No imaginary "Marketing Mary."**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Persona Extract transforms decision-log output and signal scan data into deep, evidence-based buyer personas using the Signal-to-Story Pipeline. It searches the web for real pain stories and success stories, maps decision points where a product could intervene, runs Bob Moesta's Four Forces analysis (Push/Pull/Anxiety/Habit), and clusters everything into 3-4 behavioral archetypes.

Every persona claim traces back to a real quote from a real person on a real platform. No hypotheticals. No "Marketing Mary, 34, likes yoga."

## Pipeline Position

```
signal-scan ───▶ decision-log ───▶ ╔═════════════════╗
                                   ║                 ║
                                   ║ persona-extract ║───▶ offer-scope ───▶ pitch ───▶ hunter-log
                                   ║   ** here **    ║
                                   ║                 ║
                                   ╚═════════════════╝
                                           │
                                           ▼
                                   ┌───────────────┐
                                   │ swot-analysis  │ (optional stress-test)
                                   └───────────────┘
```

## Input

| Field | Source | Required |
|---|---|---|
| **Decision-log output** | Domain, opportunity title, supporting signal data | Yes |
| **Pain signals** | PAIN signals from upstream signal scan | Yes |
| **Spend signals** | SPEND signals for WTP anchoring | Yes |
| **Behavior signals** | BEHAVIOR signals from upstream | Yes |
| **Ship candidates** | Candidate products from scan or decision | Yes |

## Output

Two files written to `Admin/Product-Discovery/Personas/`:

| File | Format | Contents |
|---|---|---|
| `persona-extract-{domain}-{date}.json` | JSON | Structured persona data validated against schema |
| `{domain}-{date}.md` | Markdown | Persona cards, pain/success stories, decision points, Four Forces, offer mapping |

## Workflow

```
Decision-Log Output + Signal Data
    │
    ├── Phase 1: Opportunity Definition ─── frame the research scope
    │
    ├── Phase 2: Evidence Collection ─── web search (Reddit, HN, DEV.to, reviews, forums)
    │
    ├── Phase 3: Pain Story Collection ─── verbatim quotes, real attributions (15-20 raw stories)
    │
    ├── Phase 4: Success Story Collection ─── what winners did differently
    │
    ├── Phase 5: Decision Point Mapping ─── forks where stuck vs. success paths diverge
    │
    ├── Phase 6: Four Forces Analysis ─── Push, Pull, Anxiety, Habit (global, 3+ entries each)
    │
    ├── Phase 7: Persona Clustering ─── 3-4 behavioral archetypes from evidence
    │
    ├── Phase 8: Offer Mapping ─── decision points → product interventions
    │
    └── Phase 9: Next-Step Hint ─── top persona + decision point + WTP for offer-scope
```

### Persona Structure

Each persona captures:

| Field | Description |
|---|---|
| **Archetype** | Memorable name encoding behavior pattern (e.g., "The Midnight Firefighter") |
| **Pain stories** | 3+ stories with situation, pain, workaround, emotional state, and real quote |
| **Success stories** | 2+ stories of what winners did differently |
| **Decision points** | Trigger → stuck behavior → success behavior → product intervention |
| **Buying triggers** | Specific events that cause search and purchase |
| **Objections** | Hesitations with evidence-based counters |
| **WTP** | Willingness to pay anchored to SPEND data |
| **Channels** | Platform + behavior + estimated reach |

### Four Forces (Moesta)

| Force | Question |
|---|---|
| **Push** | What is wrong with the current situation? |
| **Pull** | What attracts them to a solution? |
| **Anxiety** | What scares them about buying/switching? |
| **Habit** | What keeps them doing nothing? |

For a switch to happen: Push + Pull must exceed Anxiety + Habit.

## Usage

```
"Extract personas for [opportunity]"
"Who is the buyer for [domain/opportunity]?"
"Run persona extraction on this decision"
"Build personas from this signal scan"
"/persona-extract [decision-log output]"
```

## Quality Checklist

- [ ] Every pain/success story has a real quote with attribution
- [ ] No hypothetical personas -- all archetypes from observed stories
- [ ] Web search performed in Phases 2-4 (cannot work from memory)
- [ ] Minimum 3 personas, each with 3+ evidenced pain stories
- [ ] Four Forces covers all 4 quadrants with 3+ entries each
- [ ] Decision points map to specific product interventions
- [ ] WTP anchored to SPEND data, not guesses
- [ ] Next-step hint populated for offer-scope
- [ ] JSON validates against schema
- [ ] Pipeline kanban updated

---

<div align="center">

**Their language IS your data.**

[![Pipeline](https://img.shields.io/badge/Prev-decision--log-8b949e?style=for-the-badge)]()
[![Pipeline](https://img.shields.io/badge/Next-offer--scope-4DCFC9?style=for-the-badge)]()

</div>
