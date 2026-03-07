<div align="center">

# 📡 Signal Scan

### Product signal detection across 7 canonical types

[![Pipeline Stage](https://img.shields.io/badge/Stage-1_of_6-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()
[![Context](https://img.shields.io/badge/Context-Fork-facc15?style=for-the-badge)]()

**Systematically scan a domain for product opportunities using real evidence, not vibes.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Signal Scan is the entry point of the Hunter Pipeline. It takes a domain, an operator's unique edge, and constraints, then systematically collects, normalizes, and scores signals across 7 canonical types using real web search data. Every signal must have evidence -- an actual quote, a specific number, a URL. No hypotheticals. No "people generally feel..."

The output is a scored opportunity ranking with concrete ship candidates, ready for the decision-log to evaluate.

## Pipeline Position

```
 ╔══════════════╗
 ║              ║
 ║ signal-scan  ║───▶ decision-log ───▶ persona-extract ───▶ offer-scope ───▶ pitch ───▶ hunter-log
 ║  ** here **  ║
 ║              ║
 ╚══════════════╝
```

## Input

| Field | Description | Required |
|---|---|---|
| **Domain** | The space to scan (e.g., "DevOps education", "AI writing tools") | Yes |
| **Operator edge** | What makes you uniquely qualified -- skills, audience, credentials | Yes |
| **Constraints** | Time-to-ship target, price range, format preferences, distribution channels | Yes |

## Output

Two files written to the Obsidian vault at `Admin/Product-Discovery/Signal-Scans/`:

| File | Format | Contents |
|---|---|---|
| `signal-scan-{domain}-{date}.json` | JSON | Structured scan data validated against `references/output-schema.json` |
| `{domain}-{date}.md` | Markdown | Executive summary, signal highlights, opportunity ranking table, ship candidates |

## Workflow

```
Domain + Edge + Constraints
    │
    ├── Phase 1: Domain Definition ─── confirm scope, boundaries, search targets
    │
    ├── Phase 2: Signal Collection ─── web search across 10+ source types (real data only)
    │
    ├── Phase 3: Normalization ─── structure every signal per its type schema
    │
    ├── Phase 4: Opportunity Scoring ─── 6 dimensions, weighted average
    │
    ├── Phase 5: Offer Generation ─── 3+ ship candidates per opportunity scoring 6+
    │
    └── Phase 6: Meta-Signal Synthesis ─── the thesis that ties it all together
```

### The 7 Signal Types

| Type | What It Measures | Key Sources |
|---|---|---|
| **PAIN** | How much does this hurt? | Reddit, forums, Discord |
| **DEMAND** | Are people actively looking? | Google Trends, search volume, job postings |
| **SPEND** | Are people already paying? | Udemy, Gumroad, AppSumo |
| **SENTIMENT** | How do people feel about current solutions? | G2, Capterra, reviews |
| **COMPETITIVE** | How bad are existing alternatives? | Competitor sites, pricing pages |
| **BEHAVIOR** | What workarounds are people using? | Stack Overflow, duct-tape solutions |
| **AUDIENCE** | Can you reach these people? | Community sizes, engagement patterns |

### Scoring Dimensions

| Dimension | Weight | Source |
|---|---|---|
| Pain intensity | 2x | PAIN signals |
| Spend evidence | 2x | SPEND signals |
| Edge match | 1x | Operator profile |
| Time to ship | 1x | Format/complexity |
| Competition gap | 1x | COMPETITIVE + SENTIMENT |
| Audience fit | 1x | AUDIENCE signals |

**Score** = `(pain*2 + spend*2 + edge + time + gap + audience) / 8`

## Usage

```
"Scan [domain] for product signals"
"What should I build in [domain]?"
"Run a signal scan on [domain]"
"Market analysis for [domain]"
"/signal-scan [domain]"
```

## Quality Checklist

- [ ] All 7 signal types have at least 2 entries (or explicit gap explanation)
- [ ] Every signal has real evidence -- quotes, numbers, URLs
- [ ] Opportunity scores have per-dimension justification
- [ ] At least 3 ship candidates per opportunity scoring 6+
- [ ] At least one ship candidate per top opportunity shippable in 1-3 days
- [ ] Meta-signal synthesizes across types, not just summarizes
- [ ] Price points grounded in SPEND evidence
- [ ] Distribution recommendations are specific and actionable
- [ ] JSON validates against schema
- [ ] Pipeline kanban updated

---

<div align="center">

**The discipline of figuring out what to build.**

[![Pipeline](https://img.shields.io/badge/Next-decision--log-4DCFC9?style=for-the-badge)]()

</div>
