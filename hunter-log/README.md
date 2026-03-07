<div align="center">

# 📒 Hunter Log

### Persistence layer for the entire pipeline

[![Pipeline Stage](https://img.shields.io/badge/Stage-Persistence-4DCFC9?style=for-the-badge)]()
[![Tier](https://img.shields.io/badge/Tier-Core-4ade80?style=for-the-badge)]()

**Format. Save. Track. Nothing more.**

[Usage](#usage) · [Input/Output](#input--output) · [Workflow](#workflow) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Hunter Log is pure I/O. It receives a `PipelineEnvelope` from any pipeline skill, validates it, renders Obsidian-compatible markdown with correct frontmatter and wikilinks, writes the file to the vault, updates the session log, and moves the kanban card. It does not analyze. It does not interpret. It only formats and saves.

Every pipeline skill calls hunter-log at the end of its workflow to persist output. It can also be called directly with a manually assembled envelope.

## Pipeline Position

```
signal-scan ──→ decision-log ──→ persona-extract ──→ offer-scope ──→ pitch
       │               │                │                  │            │
       └───────────────┴────────────────┴──────────────────┴────────────┘
                                        │
                                        ▼
                               ╔═══════════════╗
                               ║               ║
                               ║  hunter-log   ║
                               ║  ** here **   ║
                               ║               ║
                               ╚═══════════════╝
                                        │
                           ┌────────────┼────────────┐
                           ▼            ▼            ▼
                      Vault files  Session logs  Kanban board
```

## Input

One thing: a `PipelineEnvelope`.

```json
{
  "skill": "signal-scan | decision-log | persona-extract | offer-scope | pitch",
  "version": "1.0",
  "session_id": "session-YYYY-MM-DD-NNN",
  "timestamp": "ISO 8601",
  "input_refs": ["upstream-artifact-id"],
  "output": { }
}
```

## Output

| Artifact | Location | Description |
|---|---|---|
| **Vault file** | `Admin/Product-Discovery/{type-folder}/{slug}.md` | Rendered markdown with frontmatter |
| **Session log** | `Admin/Product-Discovery/Sessions/{session-id}.md` | Artifact list, step tracking |
| **Kanban card** | `Admin/Product-Discovery/Pipeline.kanban.md` | Card created or moved to target column |

### Folder Mapping

| Skill | Target Directory | Kanban Column |
|---|---|---|
| signal-scan | `Signal-Scans/` | Signal Scanned |
| decision-log | `Decisions/` | Decision Made |
| persona-extract | `Personas/` | Persona Researched |
| offer-scope | `Offers/` | Offer Scoped |

### Collision Handling

If a file already exists at the resolved path, hunter-log does NOT overwrite. It appends under a new `## Update: {ISO timestamp}` heading and updates the frontmatter `status` if applicable.

## Workflow

```
PipelineEnvelope (JSON)
    │
    ├── Phase 1: Envelope Parsing ─── validate skill, version, session_id, output
    │
    ├── Phase 2: File Path Resolution ─── map skill → directory, generate slug, check collisions
    │
    ├── Phase 3: Frontmatter Generation ─── schema-driven per document type
    │
    ├── Phase 4: Body Rendering ─── markdown from JSON, wikilinks, tables
    │
    ├── Phase 5: Write to Vault ─── create or append
    │
    ├── Phase 6: Session Log Update ─── create or append, track pipeline progress
    │
    └── Phase 7: Kanban Board Update ─── move or create card in Pipeline.kanban.md
```

### Session Tracking

Session files at `Sessions/{session_id}.md` track which pipeline steps have completed, which remain, and link to all artifacts produced in that session. When all steps complete, status flips to `complete`.

## Usage

```
"Save this to the vault"
"Log this pipeline output"
"Persist the scan results"
"/hunter-log {envelope}"
"Write this decision to Obsidian"
```

## Quality Checklist

- [ ] Envelope validates against `references/pipeline-envelope-schema.json`
- [ ] Frontmatter matches the schema for the document type
- [ ] All wikilinks use correct relative paths within `Product Discovery/`
- [ ] Tags follow `hunter/` hierarchy
- [ ] Session log created or updated with new artifact
- [ ] Kanban card moved to correct column (or created if new)
- [ ] Existing files appended with timestamp heading, not overwritten
- [ ] No data invented -- only what the envelope output contains

---

<div align="center">

**The vault is the database. Hunter-log is the ORM.**

[![Pipeline](https://img.shields.io/badge/Role-Persistence-4DCFC9?style=for-the-badge)]()

</div>
