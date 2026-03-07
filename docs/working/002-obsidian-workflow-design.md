# Hunter Pipeline: Obsidian Persistence Layer Design
**Date**: 2026-02-08
**Status**: Design complete, not yet implemented

## Overview

Design for saving structured product discovery sessions to Obsidian vault at `/Users/peleke/Documents/Vaults/ClawTheCurious/`.

## Pipeline Architecture

Each step is a skill with JSON-serializable inputs/outputs:

```
signal-scan → decision-log → persona-extract → offer-scope
                    ↓              ↓               ↓
              [hunter-log: persistence layer writes to Obsidian]
```

## Vault Directory Structure

```
ClawTheCurious/
  Product Discovery/          # (already created)
    Pipeline.kanban.md        # Master kanban board
    Plan.md                   # Index page with dataview queries
    Signal Scans/             # One file per domain scan
    Decisions/                # One file per decision
    Personas/                 # One file per persona deep-dive
    Offers/                   # One file per offer spec
    Sessions/                 # Session logs tying pipeline runs together
```

## Frontmatter Schemas

### Signal Scan
```yaml
type: signal-scan
domain: "DevOps Education"
date: 2026-02-08
session: "session-2026-02-08-001"
top_opportunity: "Production Design Patterns"
top_score: 9.0
status: complete
tags: [hunter/scan, hunter/domain/devops-education]
```

### Decision Log
```yaml
type: decision
date: 2026-02-08
session: "session-2026-02-08-001"
chosen_opportunity: "Production Design Patterns"
alternatives_considered: [...]
confidence: high
revisit_by: 2026-03-08
status: active
signal_scan: "devops-education-2026-02-08"
tags: [hunter/decision, hunter/domain/devops-education]
```

### Persona Research
```yaml
type: persona
date: 2026-02-08
session: "session-2026-02-08-001"
persona_name: "Mid-Level DevOps Engineer"
pain_intensity: 8
willingness_to_pay: high
tags: [hunter/persona, hunter/domain/devops-education]
```

### Offer Spec
```yaml
type: offer-spec
date: 2026-02-08
product_name: "The DevOps Decision Guide"
format: PDF
price_point: "$29-49"
ship_time: "1 day"
status: spec
tags: [hunter/offer, hunter/domain/devops-education]
```

## JSON Interfaces Between Skills

### PipelineEnvelope (wraps all outputs)
```typescript
interface PipelineEnvelope {
  skill: "signal-scan" | "decision-log" | "persona-extract" | "offer-scope"
  version: "1.0"
  session_id: string
  timestamp: string
  input_refs: string[]
  output: SkillOutput
}
```

### Key Design Decisions
- Append-not-overwrite: existing files get `## Update: {timestamp}` sections
- hunter-log is a separate persistence skill (single responsibility)
- Hierarchical tags (`hunter/domain/devops-education`) for clean tag pane
- Wikilinks for cross-references (powers graph view)
- Denormalized frontmatter for fast dataview queries

## Skills to Build
1. `hunter-log` — persistence layer (takes any PipelineEnvelope, writes to Obsidian)
2. `decision-log` — records decisions with rationale, constraints, alternatives
3. `persona-extract` — deep persona research with pain stories, decision triggers, objections
4. `offer-scope` — produces 1-day build specs from persona data

Full design details: see background agent output or ask for the full document.
