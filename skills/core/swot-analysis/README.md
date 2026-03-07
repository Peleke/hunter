<div align="center">

# 🔍 SWOT Analysis

### Evidence-grounded hypothesis stress test with verdict

[![Pipeline Stage](https://img.shields.io/badge/Stage-Optional-facc15?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Validation-e88072?style=for-the-badge)]()
[![Context](https://img.shields.io/badge/Context-Fork-facc15?style=for-the-badge)]()

**Should you actually build this? The brutally honest answer.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

SWOT Analysis takes upstream pipeline data -- persona extraction, signal scan, decision log, and a concrete market hypothesis -- and produces an evidence-grounded Strengths/Weaknesses/Opportunities/Threats analysis. Every point cites real evidence from web research. The output includes a clear verdict (proceed / proceed-with-modifications / pivot / kill), a risk registry with monitoring thresholds, and a realistic moat assessment using Helmer's 7 Powers.

This skill runs between persona-extract and offer-scope. If the verdict is "kill," the hypothesis does not proceed to offer-scope. Optimism bias is the enemy here.

## Pipeline Position

```
signal-scan ───▶ decision-log ───▶ persona-extract ───▶ ╔═══════════════╗ ───▶ offer-scope ───▶ pitch
                                                        ║               ║
                                                        ║ swot-analysis ║
                                                        ║  ** here **   ║
                                                        ║  (optional)   ║
                                                        ╚═══════════════╝
```

The SWOT verdict determines whether the hypothesis proceeds to offer-scope at all.

## Input

| Field | Source | Required |
|---|---|---|
| **Market hypothesis** | Product, format, price, audience, distribution, brand thesis | Yes |
| **Persona data** | Pain stories, triggers, objections, WTP, channels | Yes |
| **Signal scan** | All 7 signal types from upstream | Yes |
| **Decision log** | Rationale and constraints for chosen opportunity | Yes |
| **Format research** | Research on the specific format being considered | Optional |

### What counts as a hypothesis

Not a hypothesis: "DevOps education"

A hypothesis: "A $29 decision-framework PDF for mid-level DevOps engineers, distributed through GitHub and r/devops, feeding into a $49/mo Skool community"

## Output

Two files written to `Admin/Product-Discovery/SWOT/`:

| File | Format | Contents |
|---|---|---|
| `{domain}-{date}.json` | JSON | Structured SWOT data validated against schema |
| `{domain}-{date}.md` | Markdown | 4 quadrants with evidence, verdict, risk registry, moat assessment |

## Workflow

```
Hypothesis + Persona + Signal Scan + Format Research
    │
    ├── Phase 1: Hypothesis Framing ─── concrete, falsifiable claim
    │
    ├── Phase 2: Strengths ─── internal advantages (credentials, format fit, white space)
    │
    ├── Phase 3: Weaknesses ─── internal vulnerabilities (cold start, churn, skill gaps)
    │
    ├── Phase 4: Opportunities ─── external tailwinds (market growth, platform shifts, AI)
    │
    ├── Phase 5: Threats ─── external headwinds (competitors, AI displacement, free floor)
    │
    ├── Phase 6: Synthesis ─── verdict: proceed / modify / pivot / kill
    │
    ├── Phase 7: Risk Registry ─── 4-8 specific risks with metrics and thresholds
    │
    └── Phase 8: Moat Assessment ─── Helmer's 7 Powers over 0-24+ month timeline
```

### Verdicts

| Verdict | Meaning | Next Step |
|---|---|---|
| **proceed** | Strong hypothesis. Ship as specified. | offer-scope |
| **proceed-with-modifications** | Sound but needs specific changes. | Apply mods, then offer-scope |
| **pivot** | Core insight valid, hypothesis as stated will fail. | Reformulate, re-run SWOT |
| **kill** | Fatal weaknesses. Do not invest build time. | Back to persona-extract or signal-scan |

### Moat Assessment

| Timeline | Typical State |
|---|---|
| **Months 0-6** | No moat. Competing on positioning and content quality. |
| **Months 6-12** | Brand recognition, early community network effects (maybe). |
| **Months 12-24** | Moderate moat if community reaches critical mass. |
| **Months 24+** | Strong defensibility if a 7 Powers dynamic materializes. |

## Usage

```
"Run a SWOT on this hypothesis"
"Stress-test this product idea"
"Should I actually build this?"
"What are the real risks here?"
"/swot-analysis [hypothesis]"
```

## Quality Checklist

- [ ] Hypothesis is concrete and falsifiable (product, format, price, audience, distribution, brand thesis)
- [ ] Minimum 4 points per quadrant, each with cited evidence
- [ ] Web search performed for EACH quadrant
- [ ] Every point has impact rating (high/medium/low)
- [ ] Weaknesses not softened, threats not minimized
- [ ] Verdict weighs weaknesses and threats honestly
- [ ] If proceed-with-modifications, specific actionable modifications listed
- [ ] Risk registry has 4-8 measurable risks with thresholds and monitoring methods
- [ ] Moat assessment references Helmer's 7 Powers by name
- [ ] JSON validates against schema

---

<div align="center">

**If 3 strengths are valid but 2 threats are existential, the threats dominate.**

[![Pipeline](https://img.shields.io/badge/Prev-persona--extract-8b949e?style=for-the-badge)]()
[![Pipeline](https://img.shields.io/badge/Next-offer--scope-4DCFC9?style=for-the-badge)]()

</div>
