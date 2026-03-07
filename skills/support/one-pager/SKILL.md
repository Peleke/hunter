---
name: one-pager
description: Generate print-ready one-pager documents from pitch output or manual input. Use when users need a branded PDF summarizing a validated product opportunity, service offering, or proposal.
metadata:
  context: fork
  openclaw:
    emoji: "\U0001F4C4"
---

# One-Pager

Generate print-ready one-pager documents (8.5" x 11") as React JSX components rendered in the browser and exported to PDF. Reads structured data from the pitch skill output or accepts manual input for standalone use.

## Step 0: Load Conventions

Before doing ANYTHING, read the shared conventions file:

```
Read ${SKILLS_DIR}/_conventions.md
```

This file defines: canonical vault path, folder-to-type mapping, frontmatter contract, valid statuses, tag hierarchy, cross-reference syntax, and the PipelineEnvelope schema. All output from this skill MUST conform to those conventions.

## When to Use

- After pitch skill completes: generate a branded one-pager from the pitch output
- Standalone: create a one-pager from scratch for any product, service, or proposal
- When a client or lead needs a concise, print-ready document summarizing an offering

## Trigger Phrases

- "Generate a one-pager for [product]"
- "Create a one-pager from the pitch output"
- "Make a branded PDF for [company/product]"
- "One-pager for [domain/opportunity]"
- "/one-pager [product] [theme]"

---

## Pipeline Position

```
signal-scan > decision-log > persona-extract > swot-analysis > offer-scope > pitch > ONE-PAGER > hunter-log
```

**Input**: Pitch output JSON from `${VAULT}/Admin/Product-Discovery/Pitches/`
**Output**: One-pager JSX + PDF to `${VAULT}/Admin/Product-Discovery/One-Pagers/`

---

## Prerequisites

1. **One-pager repo**: `~/Documents/Projects/one-pager/one-pager/` must exist with `npm install` completed
2. **Pitch output** (pipeline mode): Read the pitch JSON from the vault
3. **Theme selection**: Ask the user or default to `portfolio`
4. **Brand info** (if not in pitch): Company name, tagline, contact email, website, location

If running standalone (no pitch data), ask for: company name, tagline, description, key points, audience, and theme.

---

## Theme System

The one-pager supports configurable themes. Three presets are included:

| Theme | Font | Palette | When to Use |
|-------|------|---------|-------------|
| `portfolio` | Inter | Coral/green on `#0d1117` | Default. Matches slidev-deck modern theme. |
| `endstation` | Space Grotesk | Teal/mint/purple on dark green | Healthcare/AI vertical. |
| `corporate` | Inter | Blue/white on charcoal | Conservative audiences. |

Theme presets are defined in: `~/Documents/Projects/one-pager/one-pager/src/themes/presets.js`

---

## Workflow

### Pipeline Mode (after pitch)

```
Pitch output in vault
    |
Phase 1: Read pitch JSON from vault
    |
Phase 2: Run pitchAdapter to map pitch fields to one-pager schema
    |
Phase 3: Select theme + brand overrides
    |
Phase 4: Write the one-pager data JSON
    |
Phase 5: Render in browser + export PDF
    |
Phase 6: Save vault reference note
    |
Output: PDF + vault note
```

### Standalone Mode (no pitch data)

```
User provides company info + content
    |
Phase 1: Gather inputs (name, tagline, description, key points, audience)
    |
Phase 2: Select theme
    |
Phase 3: Use promptBuilder to generate one-pager schema via LLM
    |
Phase 4: Render in builder UI for preview/editing
    |
Phase 5: Export PDF
    |
Output: PDF
```

---

### Phase 1: Read Pitch Data (Pipeline Mode)

Glob the vault for the most recent pitch output:

```
Glob: ${VAULT}/Admin/Product-Discovery/Pitches/{product-slug}*.md
```

Read the pitch output and extract the JSON. The pitch output schema is defined in:

```
Read ${SKILLS_DIR}/pitch/references/output-schema.json
```

### Phase 2: Map Pitch to One-Pager Schema

Use the `pitchAdapter.js` module to convert pitch output to the one-pager renderer schema:

```js
import { pitchToOnePager } from './src/pipeline/pitchAdapter';

const onePagerData = pitchToOnePager(pitchJSON, {
  theme: 'portfolio',      // or 'endstation', 'corporate'
  brand: {
    name: 'Company Name',
    tagline: 'Tagline',
    contact: 'email@example.com',
    website: 'example.com',
    location: 'City, State',
  },
});
```

The adapter maps:
- `landing_page.headline` -> Hero section (split into before + accent)
- `landing_page.subheadline` -> Hero subtitle
- `landing_page.problem_section` -> Feature box
- `landing_page.whats_inside` -> Cards (top 3)
- `landing_page.pricing_section` -> Banner stats
- `landing_page.social_proof_strategy` -> Credentials
- `landing_page.primary_cta` -> Footer CTA
- Pipeline refs (signal_scan_ref, persona_ref, etc.) -> Credentials validation items

### Phase 3: Select Theme + Brand

Ask the user which theme to use. If not specified, default to `portfolio`.

Apply brand overrides if the user provides them (logo, company name, contact info).

### Phase 4: Write One-Pager Data

Write the generated schema JSON to the one-pager project:

```
Write ~/Documents/Projects/one-pager/one-pager/src/data/generated.json
```

### Phase 5: Render and Export

Start the dev server and open the browser:

```bash
cd ~/Documents/Projects/one-pager/one-pager && npm run dev
```

Then use browser print (Cmd+P) to export as PDF with:
- Margins: None
- Background graphics: Enabled
- Paper size: Letter (8.5" x 11")

Save the PDF to: `${VAULT}/Admin/Product-Discovery/One-Pagers/{product-slug}-one-pager-{YYYY-MM-DD}.pdf`

### Phase 6: Save Vault Reference Note

Write an Obsidian note to track the generated one-pager:

**Path**: `${VAULT}/Admin/Product-Discovery/One-Pagers/{product-slug}-one-pager-{YYYY-MM-DD}.md`

**Frontmatter**:
```yaml
---
type: one-pager
date: YYYY-MM-DD
status: draft
product: PRODUCT NAME
theme: portfolio | endstation | corporate
pitch_ref: "{pitch-slug}"
tags:
  - hunter/one-pager
  - hunter/domain/{domain-slug}
---
```

---

## Section Types

The one-pager renderer supports 8 section types:

| Type | Purpose | Key Props |
|------|---------|-----------|
| `hero` | Opening headline with accent text | headingBefore, headingAccent, subtitle |
| `banner` | Key stats/metrics row | items[].label, value, sub, accentColor |
| `steps` | Numbered process flow | sectionTitle, items[].step, title, desc |
| `cards` | Side-by-side info cards | sectionTitle, columns, items[].title, desc |
| `bullets` | Bulleted list in columns | sectionTitle, columns, bulletColor, items[] |
| `credentials` | Key-value credential pairs | sectionTitle, borderColor, items[].label, desc |
| `feature-box` | Highlighted concept with badge | badge, title, description |
| `footer` | CTA with contact button | heading, subtext, ctaText |

---

## Quality Checklist

- [ ] One-pager fits on a single 8.5" x 11" page (no overflow)
- [ ] All text is concise and benefit-oriented
- [ ] Theme colors are consistent (no off-palette colors)
- [ ] Hero headline is compelling (uses persona pain language if from pipeline)
- [ ] CTA is specific and actionable
- [ ] Brand info (logo, contact) is correct
- [ ] PDF export has background graphics enabled
- [ ] Vault reference note written with correct frontmatter

---

## Auto-Persist

After all phases complete: wrap output in PipelineEnvelope, invoke `hunter-log` to persist the vault note and update the kanban board. Do NOT ask for permission.
