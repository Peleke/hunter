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

### Phase 4.5: Narrative Layer

**This phase transforms slides from a data dump into a story people feel in their bodies.**

The difference between a forgettable deck and one that gets funded: forgettable decks present information. Funded decks make the audience *become* someone with a problem, *feel* the pain of that problem, and *see* themselves in the solution. This is neuroscience, not opinion (Warnick et al., 2018: displayed founder passion → neural engagement → investor interest).

**Core principle**: Don't tell the audience about a market. Introduce them to a person. Make them *be* that person for 15 minutes. Then show them the gap between where that person is and where they could be.

References:
- Neuroscience of pitching: passion → neural engagement → investor interest (Warnick et al., PMC4445577)
- YC: "How to Design a Better Pitch Deck" — story-driven, not data-driven
- SPIN Selling: diagnose pain through questions, never pitch features first
- Gap Selling (Keenan): sell the gap between current state and future state
- Aegir outline-writer: starting state → war story → tension → resolution → bridge

---

#### Step 1: Anchor Persona

**Before anything else, choose a protagonist.**

Pick ONE persona from the pipeline's persona-extract output. This person is the emotional anchor for the entire deck. Every slide either shows their pain, validates their pain, or solves their pain.

Write the persona card:

```
ANCHOR PERSONA: Maya
Role: Content lead at a 30-person SaaS startup
Current state: Drowning. 5 tools. Manual reshaping. Publishing slop she hates.
Emotional state: Ashamed of what she publishes. Afraid her CEO will notice.
The gap: She needs a full-loop pipeline. She doesn't know it exists.
What she'd say: "I spend 4 hours a week turning blog posts into LinkedIn carousels and I hate every second of it."
```

**Why this matters (neuroscience)**: Investors don't fund markets. They fund the *feeling* of a market — the visceral recognition of "oh, I know someone like Maya." Displayed passion about a real person's real pain activates mirror neurons. Abstract market data does not. (Chen et al., 2009; Warnick et al., 2018)

**Rules:**
- The persona must be drawn from REAL quotes in the signal scan (not invented)
- Use their actual words from Reddit/community sources where possible
- Give them a name (even if fictional) — named characters create 2.5x more neural engagement than abstractions
- The persona's pain must map directly to the product's solution

---

#### Step 2: Story Arc (Persona-Anchored)

The story arc is NOT about your product. It's about the persona's journey from pain to possibility. The product enters the story late — only after the audience is already rooting for the persona.

**The SPIN Arc** (adapted from SPIN Selling for decks):

```
Situation  → "This is Maya. Here's her world."
Problem    → "Here's what's broken. Here's the cost."
Implication → "Here's how bad it gets if nothing changes."
Need-Payoff → "Here's what her world looks like when it's fixed."
```

Every deck follows this arc. The specific implementation varies by deck type:

| Deck Type | S (Situation) | P (Problem) | I (Implication) | N (Need-Payoff) |
|-----------|--------------|-------------|-----------------|-----------------|
| **Pitch** | Meet Maya. Her world. | The fragmentation. The cost. | What happens if nobody fixes this. Evidence wall. | The full loop. Revenue. Proof it works. |
| **Validation** | The market we scanned. | What we found (pain signals). | How deep the problem goes (scoring, quotes). | What we're building and why we'll win. |
| **Technical** | The system before. | Where it breaks. | What that costs (latency, failures, $). | The architecture that fixes it. Trade-offs. |

Write the SPIN map for your deck:

```
S: "Maya runs content for a 30-person SaaS. She uses Jasper + Taplio + Castmagic + a Google Sheet."
P: "None of them talk to each other. She spends 4 hours reshaping each piece. Her CEO thinks AI handles it."
I: "65% of AI agent tasks fail (Salesforce). $2K/mo on tools that produce slop she's embarrassed to publish."
N: "A pipeline that does the full loop: research → write → reshape → publish → measure → iterate. Open-source, so she owns it."
```

---

#### Step 3: Beat Map (Emotional Sequence)

Map every slide to a **beat** — NOT "what's on the slide" but "what the audience should FEEL."

The beat sequence must follow the **tension curve** from the aegir outline-writer: build tension through the first half, deliver the turn at the midpoint, build conviction through the second half.

```
TENSION BUILDING (audience should feel increasingly uncomfortable)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Slide 1:  INTRIGUE     — "Who is this person? This feels specific."
Slide 2:  RECOGNITION  — "Oh. I know someone like Maya. Maybe I AM Maya."
Slide 3:  DISCOMFORT   — "Shit, the numbers are worse than I thought."
Slide 4:  EMPATHY      — "These are real people saying this. I feel it."

THE TURN (the moment the story pivots from pain to possibility)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Slide 5:  RESPECT      — "They scored this. This isn't vibes — it's data."

CONVICTION BUILDING (audience should feel increasingly excited)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Slide 6:  CLARITY      — "I see exactly who wants this."
Slide 7:  INSIGHT      — "I understand why they'd switch."
Slide 8:  RELIEF       — "There's a plan. It's not just an idea."
Slide 9:  CONFIDENCE   — "They know who else is trying. They've mapped the field."
Slide 10: TRUST        — "They have kill criteria. They're not delusional."
Slide N:  URGENCY      — "I need to be part of this / I need to act."

CLOSING (circular — callback to the persona)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Last:     RESOLUTION   — "Maya's world after. The pipeline that built this deck."
```

**Rules:**
- Adjacent slides CANNOT have the same beat. If they do, cut or combine.
- The emotional step between beats must be walkable — no jumping from INTRIGUE to URGENCY.
- The persona must be referenced at minimum 3 times: opening (introduce), middle (their specific pain), closing (their transformation).
- The TURN slide is the most important slide in the deck. It's where "this is bad" becomes "but there's a way."

---

#### Step 4: Transition Lines

The most important words in the deck are NOT on any slide. They're the words the speaker says BETWEEN slides. These are the connective tissue that turns a slideshow into a story.

**Write every transition as a persona callback or a question:**

```
[Slide 1 → Slide 2]
"So that's the product. But let me tell you about someone who needs it. Let me tell you about Maya."

[Slide 2 → Slide 3]
"Maya's not alone. We didn't cherry-pick her — we went looking for how widespread this really is. Here's what we found."

[Slide 3 → Slide 4]
"Those are the numbers. But numbers don't bleed. Let me show you what this sounds like when real people talk about it."

[Slide 4 → Slide 5]  ← THE TURN
"Three voices, three angles, same pain. So we asked ourselves: is this real enough to build for? We built a scoring framework to find out."

[Slide 5 → Slide 6]
"54 out of 60. Highest score the pipeline has produced. So who exactly is the customer? We found four."

[Last-1 → Last]
"Remember Maya? [pause] This deck was built by the same pipeline we're selling her. The pipeline ran on itself. If you're wondering whether it works — you just watched the proof."
```

**Transition rules (tightened):**
1. **Never say "next slide" / "moving on" / "let's look at"** — the audience should forget they're watching slides
2. **Every transition either references the persona by name OR asks a question** — no naked segues
3. **The TURN transition is the longest** — max 3 sentences. All others: max 2.
4. **Vary register**: conspiratorial ("Now here's what nobody talks about."), vulnerable ("Honestly, we almost killed this."), punchy ("Here's the kicker."), reflective ("So what does this actually mean for Maya?")
5. **The closing transition MUST callback to Slide 1** — circular stories are 22% more memorable (Stanford research)
6. **Include delivery cues in brackets**: [pause], [lower voice], [lean forward], [click to reveal]

---

#### Step 5: Slide Personality Review (SPIN Check)

Review each slide against the **Gap Selling test**: does this slide make the audience feel the gap between the persona's current state and their possible future state?

For each slide, check:

1. **Whose story is this?** — If you removed the persona's name, would this slide work in any generic pitch deck? If yes, it's too abstract. Make it specific to the persona.
2. **What's the question?** (SPIN) — Every slide should implicitly ask a question the audience is thinking. If there's no implicit question, the slide is dead air.
3. **What's the pain?** — Even solution slides should reference the pain they solve. "Revenue Model" → "When Maya stops spending $2K/mo on slop, here's where that money goes instead."
4. **What's the specificity?** — Replace every instance of "users", "customers", "the market" with the persona's name or a specific number. "65% of AI agent tasks fail" > "most agents struggle."
5. **What's the voice?** — Read the slide headline out loud. Does it sound like a human talking, or a template? "Revenue Model" → "When the Money Shows Up." "Competitive Map" → "Why Nobody Else Can Do This."
6. **Where's the callback?** — Can this slide reference something from an earlier slide? ("Remember the $2K/mo stat? Here's who's paying it.")

---

#### Step 6: Story Audit

Before proceeding to speaker notes:

- [ ] **One-sentence test**: Can I summarize the deck in one sentence that includes the persona's name? ("Maya spends $2K/mo on content tools that produce slop — we built the full-loop pipeline that fixes it, and this deck is the proof.")
- [ ] **Persona count**: Does the persona appear by name in at minimum 3 slides (open, middle, close)?
- [ ] **Pain-first check**: Does the audience feel pain before they see the solution? (Solution should not appear before slide 6 at earliest.)
- [ ] **SPIN sequence**: Do the slides follow Situation → Problem → Implication → Need-Payoff without breaking order?
- [ ] **Circular check**: Does the closing callback to the opening? (Best decks end where they began, transformed.)
- [ ] **Deletion test**: For each slide, ask "if I cut this, does the story still work?" If yes, cut it.
- [ ] **Mirror test**: Will the audience see THEMSELVES in the persona? (If the persona is too niche, they won't. If too generic, they won't care.)
- [ ] **3-slide resilience**: Could someone who missed 3 random slides still follow the story?

---

### Phase 5: Add Speaker Notes

Speaker notes are the **performance script** — the actual words the speaker says, including delivery cues, persona references, and emotional beats. They are NOT slide summaries.

Every slide MUST have speaker notes as HTML comments:

```md
# Slide Title

content

<!--
[PERSONA THREAD]: Maya — introduce / deepen / callback / resolve

[TRANSITION IN]: "So that's the landscape. But let me tell you who actually feels this. Her name is Maya."

[BEAT]: RECOGNITION — audience should think "I know someone like this."

[THE GAP]: Current state: 5 tools, manual reshaping, publishing slop.
           Future state: One pipeline, ambient content, proud of every post.
           This slide shows: current state (pain side of the gap).

[TALKING POINTS]:
- [lean forward] "Maya runs content for a 30-person SaaS. She uses five different tools that don't talk to each other."
- "Her CEO thinks AI handles content. It doesn't. Maya handles content. AI just makes more work."
- [pause] "She spends four hours a week turning blog posts into LinkedIn carousels. And she hates every single one she publishes."
- [lower voice] "She told us: 'I'm ashamed of what we put out there.'"

[TRANSITION OUT]: "Maya's not alone. We went looking for how widespread this is. [click] Here's what we found."

[DATA SOURCE]: persona-extract → personas[0].pain_stories, signal-scan → signals[type=PAIN]
[IF ASKED]: "Is Maya a real person?" → "Maya is a composite of 12 interviews and 40+ Reddit threads. Every quote attributed to her is verbatim from a real source."
-->
```

Speaker notes structure:
1. **PERSONA THREAD** — which phase of the persona arc this slide serves (introduce / deepen / callback / resolve)
2. **TRANSITION IN** — exact words to say arriving at this slide, referencing persona or posing a question
3. **BEAT** — emotional function from the beat map
4. **THE GAP** — current state vs. future state, and which side this slide shows
5. **TALKING POINTS** — 2-4 bullets with delivery cues: [pause], [lean forward], [lower voice], [click to reveal], [make eye contact]
6. **TRANSITION OUT** — exact words leaving this slide, connecting to the next
7. **DATA SOURCE** — pipeline artifact + field
8. **IF ASKED** — prepared answers to likely audience questions

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

### Narrative Quality (Phase 4.5)
- [ ] Story arc defined (Discovery / Problem-Solution / Hero's Journey)
- [ ] Beat map complete — every slide has an emotional function
- [ ] No two adjacent slides have the same beat
- [ ] Transition lines written for every slide boundary
- [ ] No transition uses "next slide" or "moving on"
- [ ] At least 2 callbacks to earlier slides exist in the second half
- [ ] One-sentence story summary passes the clarity test
- [ ] Story audit questions all answered affirmatively
- [ ] Speaker notes use the structured format (TRANSITION IN/BEAT/TALKING POINTS/TRANSITION OUT/DATA SOURCE)

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
