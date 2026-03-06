---
name: slidev-deck
description: Generate product-oriented slide decks using sli.dev with embedded hand-drawn SVGs. Use when users ask for pitch decks, strategy presentations, validation summaries, or any visual deck that tells a product/engineering story.
metadata:
  context: fork
  openclaw:
    emoji: "\U0001F3AC"
---

# Slidev Deck

Generate complete, ready-to-present slide decks using sli.dev (Markdown-based presentations) with inline hand-drawn SVGs in the portfolio's sketchy style. Each deck tells a visual story -- SVGs carry the narrative, text stays minimal. Decks pull structured data from the hunter product-discovery pipeline and produce a single `slides.md` file runnable with `npx slidev`.

## Step 0: Load Conventions

Before doing ANYTHING, read the shared conventions file:

```
Read ${SKILLS_DIR}/_conventions.md
```

This file defines: canonical vault path, folder-to-type mapping, frontmatter contract, valid statuses, tag hierarchy, cross-reference syntax, and the PipelineEnvelope schema. All output from this skill MUST conform to those conventions. If there is a conflict between this SKILL.md and `_conventions.md`, the conventions file wins.

## When to Use

- Creating a pitch deck for external audiences (warm leads, partners, potential customers)
- Summarizing hunter pipeline validation results for internal review
- Presenting technical architecture to engineering audiences
- Building strategy decks for business positioning and revenue planning
- Any situation where a visual presentation needs to tell a product or engineering story

## Trigger Phrases

- "Build a pitch deck for [product]"
- "Create a validation summary deck from [pipeline outputs]"
- "Make a strategy presentation for [product/domain]"
- "Generate a technical architecture deck for [system]"
- "Slide deck for [topic]"
- "/slidev-deck [type] [product]"

---

## Prerequisites

Before starting, establish:

1. **Deck type** -- One of: `product-pitch`, `validation-summary`, `technical-architecture`, `strategy` (see Deck Types below)
2. **Product/domain** -- What product, system, or domain the deck is about
3. **Audience** -- Who will see this (warm leads, investors, internal team, engineers, etc.)
4. **Pipeline data** -- Which pipeline outputs are available (signal scans, decisions, personas, offers, pitches). Glob the vault to find them:
   ```
   Glob: Admin/Product-Discovery/**/*.md
   ```
5. **Key message** -- The single sentence the audience should remember after the deck

If the user provides only a product name, ask about deck type, audience, and key message before proceeding. If pipeline data exists in the vault, load it automatically.

---

## Theme Selection

Decks support configurable visual themes. Ask the user which theme to use, defaulting to **modern**.

| Theme | Style | Font | When to Use |
|-------|-------|------|-------------|
| `modern` | Clean geometric, corporate | Inter / Helvetica | Default. Pitch decks, strategy, client-facing. |
| `sketchy` | Hand-drawn, napkin-on-desk | Caveat / cursive | Portfolio pieces, casual internal reviews. |

Load the selected theme:

```
Read ${SKILLS_DIR}/slidev-deck/references/themes/{theme}.md
```

The theme file defines: color palette, typography, SVG primitives (shapes, arrows, cards), sli.dev `<style>` block, and the per-SVG checklist. Follow it exactly.

**Key constraints (all themes):**
- Dark palette: `#0d1117` background, `#e88072` accent
- ViewBox `0 0 672 NNN`, no fixed width/height
- No background rect (transparent, sits on dark slide)
- Numbered callouts: coral circle + white bold number

For the base sli.dev frontmatter and slide separator conventions, see:

```
Read ${SKILLS_DIR}/slidev-deck/references/slidev-theme-config.md
```

---

## Deck Types

<!-- ref: references/deck-templates.md -->

Four deck types with full slide sequences. Load on demand:

```
Read ${SKILLS_DIR}/slidev-deck/references/deck-templates.md
```

| Type | Audience | Length | Story Arc |
|------|----------|--------|-----------|
| `product-pitch` | Warm leads, partners, investors | 14-18 slides | Problem > Evidence > Solution > Proof > Pricing > CTA |
| `validation-summary` | Internal team, advisors | 12-16 slides | Opportunity > Signals > Decision > Personas > Offer > Go/No-Go |
| `technical-architecture` | Engineers, reviewers | 12-16 slides | Problem > Architecture > Decisions > Trade-offs > Results |
| `strategy` | Stakeholders, leadership | 14-18 slides | Market > Positioning > Revenue > Execution > Kill Criteria |

---

## Workflow

```
User request (deck type + product + audience)
    |
Phase 1: Gather pipeline data (glob vault, read artifacts)
    |
Phase 2: Map data to slide sequence (fill each slide's data fields)
    |
Phase 3: Generate SVGs (use sketches skill style, inline in markdown)
    |
Phase 4: Assemble slides.md (frontmatter + global styles + slides)
    |
Phase 5: Add speaker notes (every slide)
    |
Phase 6: Quality check (run checklist)
    |
Phase 7: Write Obsidian summary note
    |
Output: slides.md + Obsidian note
```

### Phase 1: Gather Pipeline Data

Glob the vault for all pipeline artifacts related to the product:

```
Glob: ${VAULT}/Admin/Product-Discovery/**/{product-slug}*.md
Glob: ${VAULT}/Admin/Product-Discovery/**/{product-slug}*.json
```

Read each artifact and extract the fields needed for the chosen deck type. If an artifact is missing, note which slides will have incomplete data and ask the user whether to proceed with what's available or wait.

### Phase 2: Map Data to Slides

For each slide in the chosen deck type's sequence, map specific pipeline data to the slide's content fields:

```
Slide N:
  - headline: [from which artifact, which field]
  - SVG description: [what to draw, what data to embed]
  - text content: [bullets, stats, quotes -- with source]
  - speaker notes: [context, evidence, talking points]
```

If a data point is not available from the pipeline, DO NOT fabricate it. Either skip the slide or mark it as "[DATA NEEDED: describe what's missing]".

### Phase 3: Generate SVGs

For each slide that needs an SVG, generate it inline following the SVG Style Contract above. Every SVG must:

1. Use the dark-theme palette (no light backgrounds)
2. Use sketchy primitives (no clean rects or lines)
3. Use Caveat font family
4. Have a `viewBox` of `0 0 672 NNN` (width 672, height varies)
5. Be self-contained (no external references)
6. Have NO background rect (transparent, sits on `#0d1117` slide background)

For complex SVGs (architecture diagrams, pipeline flows, competitive maps), use the `sketches` skill's technique reference:

```
Read ${SKILLS_DIR}/sketches/references/svg-patterns.md
```

Adapt the whiteboard palette patterns to the dark palette colors.

### Phase 4: Assemble slides.md

Combine all slides into a single `slides.md` file:

```md
---
theme: default
title: "DECK TITLE"
info: |
  DECK DESCRIPTION
class: text-white
drawings:
  persist: false
transition: fade-out
mdc: true
---

# Slide 1 Title

content

<style>/* GLOBAL STYLES -- only on first slide */</style>

---

# Slide 2 Title

content

---
layout: two-cols
---

# Slide 3 Left

::right::

Right content

---

<!-- ... remaining slides ... -->
```

Slides are separated by `---` on their own line. Layout directives go in the slide's frontmatter block (the `---` block at the top of each slide).

### Phase 5: Add Speaker Notes

Every slide MUST have speaker notes. Add them as HTML comments at the end of each slide:

```md
# Slide Title

content

<!--
Speaker notes here. These appear in presenter view.
- Key talking point 1
- Key talking point 2
- Data reference: [which pipeline artifact, which field]
-->
```

Speaker notes should:
- Provide context the slide doesn't show
- Include exact data references (which pipeline artifact, which field)
- Suggest transitions to the next slide
- Anticipate audience questions
- Never exceed 6 bullet points

### Phase 6: Quality Check

Run the full checklist (see Quality Checklist below) before delivering.

### Phase 7: Write Obsidian Summary Note

After generating the deck, write a summary note to the vault.

---

## Pipeline Integration

### Data Extraction Map

This table shows which pipeline output fields feed which deck elements:

| Pipeline Artifact | Key Fields to Extract | Used In (Deck Slides) |
|---|---|---|
| `signal-scan` | `signals[type=PAIN]` (quotes, intensity) | Problem, Evidence, Social Proof |
| `signal-scan` | `signals[type=SPEND]` (prices, platforms) | Revenue, Pricing, Market Size |
| `signal-scan` | `signals[type=COMPETITIVE]` (competitors, gaps) | Competitive Landscape |
| `signal-scan` | `signals[type=DEMAND]` (volume, trends) | Market Size, Evidence |
| `signal-scan` | `meta_signal` | Market Thesis, Why Now |
| `signal-scan` | `opportunities[]` (ranked, scored) | Opportunity Ranking |
| `decision-log` | `decision_frame` (question, horizon, metric) | Decision Frame |
| `decision-log` | `sdp_scores` (6 dimensions, justifications) | SDP Scoring |
| `decision-log` | `bias_check` (pass/flag, notes) | Bias Check |
| `decision-log` | `pre_mortem` (failure scenarios) | Risk Analysis, Pre-Mortem |
| `decision-log` | `kill_criteria` (measurable, time-bounded) | Kill Criteria |
| `decision-log` | `thesis` | Market Thesis |
| `persona-extract` | `personas[].pain_stories` | Who Feels This, Problem |
| `persona-extract` | `personas[].four_forces` | Persona Deep-Dive |
| `persona-extract` | `personas[].decision_triggers` | Who Feels This |
| `persona-extract` | `personas[].profile` (role, context) | Target Customer |
| `offer-scope` | `product` (name, format, price) | Solution, Pricing |
| `offer-scope` | `value_equation` (Hormozi scores) | Offer Spec |
| `offer-scope` | `revenue_model` | Revenue Model |
| `offer-scope` | `distribution_plan` | Distribution Strategy |
| `offer-scope` | `execution_calendar` | Execution Calendar |
| `offer-scope` | `kill_criteria` | Kill Criteria |
| `pitch` | `landing_page_copy` | Positioning, CTA |
| `pitch` | `launch_posts` | Distribution |
| `pitch` | `positioning` | Positioning Statement |
| `pitch` | `email_sequence` | (appendix if needed) |

### Missing Data Protocol

If a pipeline artifact is missing:

1. Check if the user has the raw data available elsewhere
2. If not, mark affected slides with `<!-- [DATA NEEDED: {artifact}.{field} -- run {skill-name} to generate] -->`
3. Generate the deck with placeholder SVGs showing "Data pending: run [skill name]" in muted text
4. List all missing artifacts in the Obsidian summary note

---

## Output

### 1. slides.md

**Location**: Written to the user's specified directory, or defaulting to:
```
{project-root}/decks/{product-slug}-{deck-type}/slides.md
```

This file is ready to run with:
```bash
npx slidev slides.md
```

No `package.json` is required -- `npx slidev` handles dependency resolution. If the user wants a persistent setup, suggest:

```bash
npm init slidev@latest
# then replace slides.md with the generated file
```

### 2. Obsidian Summary Note

**Vault path**: `${VAULT}/Admin/Product-Discovery/Decks/{product-slug}-{deck-type}-{YYYY-MM-DD}.md`

Create the `Decks` folder if it doesn't exist:
```bash
mkdir -p "${VAULT}/Admin/Product-Discovery/Decks"
```

**Frontmatter**:
```yaml
---
type: deck
date: YYYY-MM-DD
status: draft
deck_type: product-pitch | validation-summary | technical-architecture | strategy
product: PRODUCT NAME
audience: TARGET AUDIENCE
tags:
  - hunter/deck
  - hunter/domain/{domain-slug}
  - hunter/opportunity/{opportunity-slug}
signal_scan_ref: "{signal-scan-slug}"
decision_ref: "{decision-slug}"
persona_ref: "{persona-slug}"
offer_ref: "{offer-slug}"
pitch_ref: "{pitch-slug}"
---
```

**Body structure**:
```markdown
# Deck: PRODUCT NAME -- DECK TYPE

**Generated**: YYYY-MM-DD
**Audience**: TARGET AUDIENCE
**Slides**: N slides
**Key message**: THE ONE SENTENCE THE AUDIENCE SHOULD REMEMBER

## Slide Outline

1. **Cover**: PRODUCT NAME
2. **The Problem**: [headline]
3. **The Evidence**: [headline]
...

## Pipeline Data Used

- Signal Scan: [[signal-scan-ref]]
- Decision Log: [[decision-ref]]
- Persona Extract: [[persona-ref]]
- Offer Scope: [[offer-ref]]
- Pitch: [[pitch-ref]]

## Missing Data

- [list any pipeline artifacts that were not available]

## File Location

`{absolute-path-to-slides.md}`

## Notes

[Any generation notes, caveats, or follow-up items]
```

---

## Quality Checklist

Run before delivering any deck:

### Content Quality
- [ ] Every slide has a single clear message (no slide tries to say two things)
- [ ] No slide has more than 6 lines of text (SVGs carry the story)
- [ ] Story arc is complete: problem --> evidence --> solution --> proof --> action
- [ ] All data traces to pipeline outputs (no fabricated stats)
- [ ] Fabricated data is flagged with `[DATA NEEDED]` markers
- [ ] Speaker notes exist on every slide
- [ ] Speaker notes reference specific pipeline artifacts

### SVG Quality
- [ ] Every key slide (problem, evidence, solution, architecture, pricing, CTA) has an inline SVG
- [ ] Total SVG count: 5-10 per deck (not fewer, not more)
- [ ] SVGs follow the dark-theme sketchy style (no clean primitives)
- [ ] No SVG uses light backgrounds (all transparent for dark slides)
- [ ] All SVGs use `viewBox` with no fixed dimensions
- [ ] All SVGs use Caveat font family
- [ ] Numbered callouts use coral fill circles with white text
- [ ] Slight rotation applied to element groups
- [ ] Colors restricted to the dark palette

### sli.dev Technical
- [ ] Frontmatter is valid YAML
- [ ] Slide separators are `---` on their own line
- [ ] Layout directives are in correct position (slide frontmatter block)
- [ ] Global style block appears only on the first slide
- [ ] `transition: fade-out` is set
- [ ] `class: text-white` is set on root frontmatter
- [ ] File is runnable with `npx slidev slides.md` without errors

### Deck Structure
- [ ] Total deck length: 12-20 slides (not more)
- [ ] Cover slide is clean (no SVG, just title + tagline)
- [ ] CTA slide is clear and actionable (one action, no ambiguity)
- [ ] Dark theme consistent throughout (no white backgrounds leak through)
- [ ] Slide transitions feel natural (each slide's speaker notes suggest a transition)

### Obsidian Output
- [ ] Summary note written to `${VAULT}/Admin/Product-Discovery/Decks/`
- [ ] Frontmatter includes all required fields (type, date, status, tags, refs)
- [ ] Slide outline in summary note matches actual deck
- [ ] Missing data documented
- [ ] File location path is absolute

---

## Auto-Persist

After all phases complete: wrap output metadata in PipelineEnvelope, invoke `hunter-log` to persist the vault note and update the kanban board. Do NOT ask for permission.
