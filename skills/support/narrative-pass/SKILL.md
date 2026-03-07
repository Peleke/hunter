---
name: narrative-pass
description: "Apply persona-anchored storytelling to decks, pitches, and landing pages. Transforms data presentations into stories that activate neural engagement. Use after slidev-deck or landing-page, or before any pitch meeting."
metadata:
  openclaw:
    emoji: "\U0001F3AD"
---

# Narrative Pass

Apply a persona-anchored narrative layer to an existing visual asset. This skill does NOT generate slides or copy — it takes existing content and transforms the *story* layer: slide order, speaker notes, transition lines, headline personality, and emotional arc.

## Step 0: Load Conventions

```
Read ${SKILLS_DIR}/_conventions.md
```

## When to Use

- After `slidev-deck` generates a deck but before presenting it
- After `landing-page` generates copy — to review narrative flow
- Before any pitch meeting — to create the speaking script
- When someone says "the deck has all the right slides but doesn't tell a story"
- When speaker notes are summaries instead of performance scripts

## Trigger Phrases

- "Run a narrative pass on the deck"
- "Write a speaking script for this presentation"
- "Make this deck tell a story"
- "Add personality to these slides"
- "This deck is a data dump — fix the narrative"

---

## Prerequisites

Before starting, the operator provides:

1. **Asset path** — the slide deck, landing page, or pitch doc
2. **Pipeline data** — which pipeline artifacts exist (signal scans, personas, etc.)
3. **Audience** — who will hear/read this (investors, customers, internal team)
4. **Context** — live presentation, async send, recorded video

If persona-extract output exists in the vault, load it. If not, the skill will build a lightweight persona from the available signal scan quotes.

---

## Workflow

### Phase 1: Anchor Persona

Choose ONE persona as the story's protagonist. This person is the emotional anchor for the entire deck.

**If persona-extract output exists:**
- Read `${VAULT}/Admin/Product-Discovery/Personas/` for the target product
- Select the persona with the highest pain intensity AND most relatable role
- Extract their actual quotes from the signal scan

**If no persona-extract output:**
- Read the signal scan's `signals[type=PAIN]` quotes
- Cluster 3-5 quotes around a common archetype
- Build a lightweight persona card

**Persona card format:**
```
ANCHOR PERSONA: [Name]
Role: [their job/context — specific, not generic]
Current state: [what their day looks like — visceral, sensory details]
Emotional state: [what they FEEL — shame, frustration, anxiety, exhaustion]
The gap: [what they need but don't have — stated in their words]
What they'd say: "[actual quote from signal scan or interviews]"
```

**Rules:**
- The persona MUST be drawn from real quotes (not invented)
- Give them a name — named characters create 2.5x more neural engagement than abstractions
- Use their actual words where possible — verbatim quotes > paraphrases
- The persona's pain must map directly to the product's solution

---

### Phase 2: SPIN Arc

Map the deck to the SPIN arc (adapted from Neil Rackham for presentations):

| Phase | What | Audience Feels | Slides |
|-------|------|---------------|--------|
| **Situation** | "This is [Persona]. Here's their world." | Recognition, familiarity | 1-2 slides |
| **Problem** | "Here's what's broken. Here's the cost." | Discomfort, "I've seen this" | 2-3 slides |
| **Implication** | "Here's how bad it gets. Here's the evidence." | Shock, empathy, "this is worse than I thought" | 2-3 slides |
| **Need-Payoff** | "Here's what their world looks like when it's fixed." | Relief, excitement, conviction | Remaining slides |

**Rules:**
- The Solution (product) does NOT appear until the Need-Payoff phase
- The audience must feel pain BEFORE they see the solution
- Each phase transition is a natural question: "How bad is it?" → "Who feels this?" → "Can anyone fix it?" → "What happens if they do?"

Write the SPIN map:
```
S: "[Persona] does [job]. They use [tools]. Their day looks like [specific details]."
P: "[Specific thing] is broken. It costs them [specific amount/time/pain]."
I: "[Evidence stat]. [Quote]. [What happens if nothing changes]."
N: "[Product] does [specific thing]. [Persona]'s day now looks like [specific details]. [Proof it works]."
```

---

### Phase 3: Beat Map

Map every slide to an emotional beat. The beat sequence follows a **tension curve**:

```
TENSION BUILDING (first half — audience gets increasingly uncomfortable)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INTRIGUE → RECOGNITION → DISCOMFORT → EMPATHY

THE TURN (midpoint — "but there's a way")
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RESPECT

CONVICTION BUILDING (second half — audience gets increasingly excited)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CLARITY → INSIGHT → RELIEF → CONFIDENCE → TRUST → URGENCY

CLOSING (circular — resolve the persona's story)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RESOLUTION
```

**Rules:**
- Adjacent slides CANNOT have the same beat
- The persona must appear by name in minimum 3 slides: open, middle, close
- The TURN is the most important slide — where "this is bad" becomes "but there's a way"
- The closing MUST callback to the opening

---

### Phase 4: Transition Lines

Write the exact words the speaker says between slides. These are the most important words in the deck.

**Every transition must either reference the persona by name OR ask a question.**

```
[Slide N → Slide N+1]
"So that's [Persona]'s world. But she's not alone. We went looking for how widespread this is. [click] Here's what we found."
```

**Rules:**
- Never say "next slide" / "moving on" / "let's look at"
- Max 2 sentences (except THE TURN: max 3)
- Include delivery cues: [pause], [lean forward], [lower voice], [click to reveal], [make eye contact]
- Vary register: conspiratorial / vulnerable / punchy / reflective
- The closing transition callbacks to Slide 1

---

### Phase 5: Slide Personality Review

For each slide, apply the **Gap Selling check**:

1. **Whose story?** — If you removed the persona's name, would this slide work in any generic deck? If yes, make it specific.
2. **What's the question?** — What is the audience thinking? If there's no implicit question, the slide is dead air.
3. **What's the gap?** — Current state vs. future state. Which side does this slide show?
4. **What's the specificity?** — Replace "users"/"customers"/"the market" with the persona's name or a number.
5. **What's the voice?** — Read the headline out loud. Human or template? "Revenue Model" → "When the Money Shows Up."
6. **Where's the callback?** — Can this slide reference an earlier slide?

**May result in:**
- Reordering slides (move solution later, move pain earlier)
- Rewriting headlines (template → personality)
- Adding persona references to slides that lack them
- Cutting slides that don't advance the story

---

### Phase 6: Speaker Notes Rewrite

Rewrite every speaker note as a **performance script**:

```
<!--
[PERSONA THREAD]: [Persona] — introduce / deepen / callback / resolve
[TRANSITION IN]: "exact words to say arriving at this slide"
[BEAT]: EMOTION — "what the audience should feel"
[THE GAP]: Current: ... → Future: ... → This slide shows: ...
[TALKING POINTS]:
- [delivery cue] "exact words"
- [delivery cue] "exact words"
[TRANSITION OUT]: "exact words leaving this slide"
[DATA SOURCE]: pipeline artifact → field
[IF ASKED]: "likely question?" → "prepared answer"
-->
```

---

### Phase 7: Story Audit

- [ ] One-sentence test (includes persona name)
- [ ] Persona appears 3+ times by name
- [ ] Pain before solution (solution after slide 5)
- [ ] SPIN sequence unbroken
- [ ] Circular close (callbacks to opening)
- [ ] Every slide passes deletion test
- [ ] Mirror test (audience sees themselves in persona)

---

### Phase 8: Bragi Prose Gate

Run `buildlog_gauntlet_rules(persona="bragi")` to load the 20-rule prose blocklist. Sweep ALL generated text — headlines, speaker notes, transition lines — against it.

**Critical violations (hard fail — rewrite before shipping):**

1. **Em-dash ban** — zero em dashes (—). Use colons, semicolons, commas, parentheses, or separate sentences.
2. **AI vocabulary blocklist** — none of: Additionally (sentence-initial), align with, crucial, delve, emphasizing, enduring, enhance, fostering, garner, highlight (verb), interplay, intricate/intricacies, key (adjective), landscape (abstract), pivotal, showcase, tapestry (abstract), testament, underscore (verb), valuable, vibrant.
3. **Inflated significance framing** — no "marking a pivotal moment", "setting the stage for", "reflects broader trends", "evolving landscape". Name the concrete thing that happened.
4. **Promotional puffery** — no "nestled in the heart of", "breathtaking", "renowned", "groundbreaking", "boasts", "showcasing", "exemplifies", "commitment to", "profound". Cut every adjective that doesn't help distinguish this subject from any other.
5. **Performative honesty** — no "To be frank:", "The truth is:", "Being honest about where things are:". Just state the thing.

**Structural violations (fix or justify):**

6. **Tricolon / five-item enumerations** — don't default to groups of three or five for rhetorical rhythm. Itemize what matters.
7. **Rhythmic parallel construction closers** — no "decisions tied to outcomes, updated by evidence, grounded in practice" sermon cadences. End with a concrete claim.
8. **Punchy fragment marketing copy** — no sentence fragments styled as ad slogans in technical context.
9. **Self-referential pivot** — no "This is where things get interesting." Make the point; don't announce it.
10. **Copula avoidance** — default to "is", "are", "has". Don't inflate to "serves as", "stands as", "represents".

**Process:**
- Read every headline aloud. If it could appear in any generic deck, rewrite it.
- Read every transition line aloud. If it sounds like a conference MC, rewrite it.
- Read every speaker note. If a sentence uses 2+ blocklist words, kill it and start over.
- The persona's actual quotes are the gold standard. When in doubt, use their words, not yours.

---

## Output

1. **Modified asset** — slides.md or landing page with rewritten headlines, reordered slides, and performance-script speaker notes
2. **Narrative brief** — standalone document with: anchor persona card, SPIN map, beat map, all transition lines, one-sentence story
3. **Obsidian note** — summary written to `${VAULT}/Admin/Product-Discovery/Decks/`

---

## Quality Checklist

### Narrative
- [ ] Anchor persona drawn from real pipeline data (not invented)
- [ ] SPIN arc follows S→P→I→N without breaking order
- [ ] Beat map has no adjacent duplicate beats
- [ ] Tension curve has a clear TURN at or near midpoint
- [ ] Transition lines written for every slide boundary
- [ ] No transition uses "next slide" / "moving on"
- [ ] At least 2 callbacks to earlier slides in the second half
- [ ] Closing callbacks to opening (circular)

### Performance Script
- [ ] Every slide has speaker notes in the structured format
- [ ] Delivery cues included ([pause], [lean forward], etc.)
- [ ] Persona thread tagged on every slide (introduce/deepen/callback/resolve)
- [ ] THE GAP documented on every slide (current vs. future state)
- [ ] IF ASKED section on slides with data claims

### Personality (Bragi Gate)
- [ ] All template-sounding headlines rewritten with voice
- [ ] Persona referenced by name 3+ times
- [ ] At least one vulnerable/honest moment ("We almost killed this.")
- [ ] Bragi prose gate passed: zero em dashes, zero blocklist words, zero puffery
- [ ] No tricolons or five-item enumerations used for rhetorical rhythm
- [ ] No self-referential pivots ("This is where it gets interesting")
- [ ] Specific numbers used instead of generalities
- [ ] Every adjective helps distinguish this subject from any other (or cut it)

---

## Auto-Persist

After all phases complete: wrap the narrative brief in PipelineEnvelope, invoke `hunter-log` to persist the vault note.
