# Deck Type Templates

## 1. Product Pitch Deck

**Audience**: Warm leads, partners, potential customers, investors.

**Story arc**: Problem --> Evidence --> Solution --> How It Works --> Proof --> Pricing --> CTA.

**Length**: 14-18 slides.

**Pipeline data sources**:
- `signal-scan` output: pain signals, demand signals, evidence quotes
- `decision-log` output: SDP scores, thesis statement
- `persona-extract` output: pain stories, decision triggers, persona names
- `offer-scope` output: pricing, value equation, distribution plan
- `pitch` output: landing page copy, launch posts, positioning

### Slide Sequence

**Slide 1: Cover**
```md
---
layout: cover
background: '#0d1117'
---

# PRODUCT NAME

TAGLINE FROM PITCH POSITIONING COPY

<style>/* global styles here */</style>
```
- No SVG on cover. Clean title + tagline.
- Speaker notes: introduce yourself, set context for the audience.

**Slide 2: The Problem**
```md
---
layout: center
---
```
- **SVG**: "Pain landscape" -- 3-4 sketchy boxes showing the top pain points from signal-scan PAIN signals. Each box has a short pain quote from real evidence. Boxes connected by dashed lines to a central "problem" label. Red (`#f87171`) accent on the most acute pain.
- **Text**: One headline sentence naming the problem.
- **Data source**: `signal-scan` PAIN signals (top 3-4 by intensity).
- Speaker notes: read the actual pain quotes, name the Reddit threads or communities where these came from.

**Slide 3: The Evidence**
```md
---
layout: center
---
```
- **SVG**: "Signal dashboard" -- A sketchy 2x3 or 3x2 grid of stat cards. Each card shows one metric: number with a label. Examples: "47 upvotes on 'I spent 4 hours...'", "$19-49 Udemy courses, 10K+ students", "3 Reddit threads same complaint". Use numbered callouts (coral circles).
- **Text**: One headline: "We didn't guess. We measured."
- **Data source**: `signal-scan` evidence quotes and metrics across all signal types.
- Speaker notes: walk through each stat, explain why this is not a guess but a measured signal.

**Slide 4: Who Feels This**
```md
---
layout: two-cols
---
```
- **SVG (left column)**: "Persona sketch" -- A hand-drawn persona card with name, role title, and 2-3 bullet pain points. Use a sketchy person icon (circle head + bezier body outline) at the top.
- **Text (right column)**: Key persona details: role, pain story snippet (in their words), decision trigger, what they've tried that failed.
- **Data source**: `persona-extract` primary persona.
- Speaker notes: tell the persona's pain story. Use their actual language.

**Slide 5: Why Now**
```md
---
layout: center
---
```
- **SVG**: "Timing diagram" -- A sketchy timeline showing 3-4 events that create the current window of opportunity. Use the numbered-circle style from the pipeline diagram. Connect events with bezier arrows. The final event (now) gets a glow filter.
- **Text**: One sentence: why this problem is acute right now and not two years ago.
- **Data source**: `signal-scan` meta-signal synthesis + `decision-log` thesis.
- Speaker notes: explain the structural shift that created this opportunity.

**Slide 6: The Solution**
```md
---
layout: center
---
```
- **SVG**: "Solution overview" -- Central product name in a glowing sketchy box, surrounded by 3-4 feature/benefit boxes connected by arrows. Each benefit box uses green (`#4ade80`) accent to contrast with the red pain boxes from slide 2.
- **Text**: One headline naming the product. No bullet lists.
- **Data source**: `offer-scope` product description and value proposition.
- Speaker notes: describe the product in one sentence, then explain each benefit area.

**Slide 7: How It Works (1 of 2)**
```md
---
layout: center
---
```
- **SVG**: "Process flow" -- A 3-5 step horizontal or vertical flow showing how the product works. Each step is a numbered coral circle with a label. Steps connected by bezier arrows. Keep it high-level (user perspective, not technical internals).
- **Text**: None or minimal -- the SVG IS the content.
- **Data source**: `offer-scope` curriculum/product structure.
- Speaker notes: walk through each step. Explain what the user does and what they get.

**Slide 8: How It Works (2 of 2) -- Optional**
```md
---
layout: two-cols
---
```
- **SVG (left)**: "Before/After" comparison -- Two-column sketchy comparison. Left side (red accents): current state pain. Right side (green accents): after-state with product. Use the abstraction-fork SVG style from the portfolio.
- **Text (right)**: 3 bullet contrasts: "Before: X. After: Y."
- **Data source**: `persona-extract` pain stories vs. `offer-scope` outcomes.
- Speaker notes: contrast the current workflow with the product workflow.

**Slide 9: Social Proof / Validation**
```md
---
layout: center
---
```
- **SVG**: "Evidence wall" -- 3-4 quote boxes arranged in a staggered layout. Each box contains a real quote from signal-scan evidence (Reddit, HN, etc.) with source attribution. Boxes have slight rotation for the napkin-on-desk feel.
- **Text**: Headline: "What they're already saying."
- **Data source**: `signal-scan` raw signal evidence quotes.
- Speaker notes: these are real quotes from real people. Name the sources.

**Slide 10: Competitive Landscape**
```md
---
layout: center
---
```
- **SVG**: "Competitive map" -- 2x2 quadrant diagram (sketchy axes, labeled). Plot competitors as labeled dots. Product occupies the desirable quadrant (upper-right or equivalent). Axes labels come from the two most important differentiation dimensions from `signal-scan` COMPETITIVE signals.
- **Text**: One sentence positioning statement.
- **Data source**: `signal-scan` COMPETITIVE signals + `decision-log` competition gap analysis.
- Speaker notes: explain why competitors miss the mark. Reference specific gaps.

**Slide 11: Revenue Model**
```md
---
layout: center
---
```
- **SVG**: "Revenue funnel" -- A sketchy funnel or stacked bar showing the revenue path: audience size --> conversion rate --> revenue per customer --> total addressable. Use green for revenue numbers. Use coral numbered circles for each stage.
- **Text**: Key number: projected revenue or price point.
- **Data source**: `offer-scope` revenue model and pricing.
- Speaker notes: walk through the math. Ground every number in evidence from the pipeline.

**Slide 12: Pricing**
```md
---
layout: center
---
```
- **SVG**: "Pricing card" -- A sketchy pricing card with product name, price, and 4-5 included items (checkmarks in green). If there are tiers, show 2-3 side-by-side cards with the recommended tier highlighted (glow filter).
- **Text**: Price and what's included. Nothing else.
- **Data source**: `offer-scope` pricing and product inclusions.
- Speaker notes: justify the price by referencing competitive pricing from SPEND signals.

**Slide 13: Risk Mitigation**
```md
---
layout: center
---
```
- **SVG**: "Kill criteria dashboard" -- 3-4 sketchy boxes, each containing one kill criterion with its metric and deadline. Use red (`#f87171`) for the thresholds, muted text for the descriptions. This shows the audience you have exit discipline.
- **Text**: Headline: "We know when to stop."
- **Data source**: `decision-log` kill criteria + `offer-scope` kill criteria.
- Speaker notes: explain each criterion. This builds trust -- you're not blindly optimistic.

**Slide 14: CTA**
```md
---
layout: center
---
```
- **SVG**: Minimal -- just a large hand-drawn arrow pointing right, with glow filter. The arrow is the visual.
- **Text**: Clear call to action. One sentence. URL or next step. Large font.
- **Data source**: `pitch` CTA copy.
- Speaker notes: tell them exactly what to do next. One action, no ambiguity.

**Slide 15: Appendix / Q&A -- Optional**
```md
---
layout: center
---

# Q&A

<!-- Speaker notes: have pipeline data ready for deep-dive questions -->
```
- No SVG. Clean slide.
- Speaker notes: reference specific pipeline artifacts for anticipated questions.

---

## 2. Validation Summary Deck

**Audience**: Internal team, co-founders, advisors.

**Story arc**: Opportunity --> Signals --> Decision --> Personas --> Offer --> Go/No-Go.

**Length**: 12-16 slides.

**Pipeline data sources**: All pipeline outputs for the product being reviewed.

### Slide Sequence

**Slide 1: Cover**
```md
---
layout: cover
background: '#0d1117'
---

# Validation Summary: PRODUCT NAME

Pipeline review -- DATE

<style>/* global styles */</style>
```
- Speaker notes: context for why we're reviewing this pipeline run.

**Slide 2: Pipeline Overview**
```md
---
layout: center
---
```
- **SVG**: "Pipeline stages" -- The 6-stage pipeline diagram (Signal Scan --> Decision Log --> Persona Extract --> SWOT --> Offer Scope --> Pitch). Numbered coral circles, sketchy boxes, bezier arrows. Highlight which stages are complete (green checkmark or green accent) vs. pending (muted). Use the exact style from the portfolio's `pipeline-architecture.svg`.
- **Text**: None.
- **Data source**: Check which pipeline artifacts exist in the vault for this product.
- Speaker notes: walk through the pipeline. Explain what each stage produces.

**Slide 3: Signal Scan Results**
```md
---
layout: center
---
```
- **SVG**: "Signal heatmap" -- A 7-row table-like visualization showing the 7 signal types (PAIN, DEMAND, SPEND, COMPETITIVE, SENTIMENT, BEHAVIOR, AUDIENCE) with a strength indicator for each (1-5 filled circles or bars). Use coral for strong signals, muted for weak.
- **Text**: Domain name and scan date.
- **Data source**: `signal-scan` signal counts and intensities.
- Speaker notes: which signals are strong, which are weak, what gaps exist.

**Slide 4: Top Signals (Detail)**
```md
---
layout: center
---
```
- **SVG**: "Evidence cards" -- 3-5 signal evidence cards in a staggered layout. Each card has: signal type label, evidence quote, source, intensity score. Most intense signal gets glow treatment.
- **Text**: None.
- **Data source**: `signal-scan` top 3-5 signals by intensity.
- Speaker notes: read each quote. Explain why it matters.

**Slide 5: Opportunity Ranking**
```md
---
layout: center
---
```
- **SVG**: "Ranked opportunities" -- A horizontal bar chart (sketchy bars) showing 3-5 opportunities ranked by score. Use coral fill proportional to score. The chosen opportunity gets a green border.
- **Text**: Table below SVG with opportunity names and scores.
- **Data source**: `signal-scan` opportunity ranking.
- Speaker notes: explain the scoring dimensions. Why the top opportunity is top.

**Slide 6: Decision Frame**
```md
---
layout: center
---
```
- **SVG**: "Decision snapshot" -- A sketchy card showing: Decision statement, time horizon, success metric, constraints. Clean layout, one item per row. Use the two-column comparison style if comparing go vs. no-go.
- **Text**: The decision statement in large text.
- **Data source**: `decision-log` Phase 1 (decision frame).
- Speaker notes: why this frame, what alternatives were considered.

**Slide 7: SDP Scoring**
```md
---
layout: center
---
```
- **SVG**: "Radar chart" -- A sketchy radar/spider chart showing 6 SDP dimensions (Pain, Spend, Edge, Speed, Gap, Audience) for the chosen opportunity. Plot as a filled polygon with coral accent. Optionally overlay the runner-up in muted gray.
- **Text**: SDP score as a large stat: "42/60".
- **Data source**: `decision-log` Phase 3 (SDP scores).
- Speaker notes: walk through each dimension. Cite the evidence for each score.

**Slide 8: Bias Check + Pre-Mortem**
```md
---
layout: two-cols
---
```
- **SVG (left)**: "Bias check" -- A sketchy checklist with 4 items (confirmation, availability, anchoring, excitement). Each has a green check or red flag. Simple layout.
- **Text (right)**: Pre-mortem scenarios: 3 numbered failure scenarios.
- **Data source**: `decision-log` Phases 5-6.
- Speaker notes: be honest about the biases. Which flags were raised and why we proceeded anyway.

**Slide 9: Persona Deep-Dive**
```md
---
layout: center
---
```
- **SVG**: "Persona profile" -- Hand-drawn persona card with: name, role, a simplified Four Forces diagram (push/pull/anxiety/habit as four quadrants around a central label). Use green for push/pull (driving change), red for anxiety/habit (resisting change).
- **Text**: None -- SVG carries all content.
- **Data source**: `persona-extract` primary persona + Four Forces analysis.
- Speaker notes: tell the persona's story. Use their actual words from pain stories.

**Slide 10: Offer Spec**
```md
---
layout: center
---
```
- **SVG**: "Offer blueprint" -- A sketchy product card showing: product name, format, price, ship time, distribution channel. Arranged as a labeled box with sections. Hormozi value equation scores shown as a small sub-diagram (dream outcome, perceived likelihood, time delay, effort/sacrifice).
- **Text**: None.
- **Data source**: `offer-scope` product spec and value equation.
- Speaker notes: explain the value equation. Why this format, this price, this channel.

**Slide 11: Kill Criteria**
```md
---
layout: center
---
```
- **SVG**: "Tripwire board" -- 3-4 sketchy card strips, each showing one kill criterion with metric, threshold, and deadline. Use red accent for thresholds. Visual metaphor: tripwires that trigger an exit.
- **Text**: Headline: "When we walk away."
- **Data source**: `decision-log` + `offer-scope` kill criteria.
- Speaker notes: commit to these out loud. This is a contract with the team.

**Slide 12: Go/No-Go Recommendation**
```md
---
layout: center
---
```
- **SVG**: A single large word -- "GO" in green with glow filter, or "NO-GO" in red with glow filter, or "CONDITIONAL" in coral. Below it, 2-3 condition bullets if conditional.
- **Text**: The recommendation, one sentence.
- **Data source**: Synthesized from all pipeline outputs.
- Speaker notes: summarize the case. Restate the key evidence. Ask for the decision.

---

## 3. Technical Architecture Deck

**Audience**: Engineering teams, technical reviewers, architecture review boards.

**Story arc**: Problem --> Architecture --> Key Decisions --> Trade-offs --> Results.

**Length**: 12-16 slides.

**Pipeline data sources**: Primarily system knowledge + any relevant pipeline outputs for product context. This deck type may be used independently of the hunter pipeline.

### Slide Sequence

**Slide 1: Cover**
```md
---
layout: cover
background: '#0d1117'
---

# SYSTEM NAME: Architecture Overview

DATE

<style>/* global styles */</style>
```

**Slide 2: The Problem**
```md
---
layout: center
---
```
- **SVG**: "Problem diagram" -- Show the system constraint or scaling challenge. Example: "stdio at scale" diagram showing 1 consumer (fine), 3 consumers (not fine), N consumers (dead). Use the portfolio's scaling diagram style.
- **Text**: One sentence naming the engineering challenge.
- Speaker notes: technical context for why this architecture exists.

**Slide 3: Architecture Overview**
```md
---
layout: center
---
```
- **SVG**: "System architecture" -- Full system diagram with layers, services, data stores, external inputs/outputs. Use the pipeline-architecture.svg style: numbered coral circles for layers, sketchy boxes, bezier arrows for data flow, dashed lines for external boundaries. This is the hero SVG of the deck -- spend the most detail here.
- **Text**: None.
- Speaker notes: walk through each layer top to bottom. Name the technologies at each layer.

**Slide 4: Data Flow**
```md
---
layout: center
---
```
- **SVG**: "Data flow diagram" -- Show how data moves through the system. Use animated-dash bezier paths (via `stroke-dasharray` + descriptions in speaker notes since slidev can add CSS animations). Label each flow with what data travels on it.
- **Text**: None.
- Speaker notes: trace a single request through the system end to end.

**Slide 5: Key Decision 1**
```md
---
layout: two-cols
---
```
- **SVG (left)**: "Decision fork" -- The abstraction-fork style: two columns (Option A vs. Option B), with trade-offs listed. Chosen option in green, rejected in red/muted.
- **Text (right)**: Decision rationale: why Option A won. 2-3 bullet reasons.
- Speaker notes: what would have happened if we'd chosen the other option.

**Slide 6: Key Decision 2**
```md
---
layout: two-cols
---
```
- Same structure as Slide 5 for the second key decision.
- Speaker notes: pattern of decisions tells a story about engineering values.

**Slide 7: Key Decision 3 -- Optional**
```md
---
layout: two-cols
---
```
- Same structure. Use only if there's a third critical decision worth presenting.

**Slide 8: The Abstraction That Paid Off**
```md
---
layout: center
---
```
- **SVG**: "Import swap" -- Show the before/after of the key abstraction. The portfolio's import-swap.svg style: two code-like boxes showing the one-line change that enabled the architecture shift. Green accent on the "after".
- **Text**: One sentence explaining the leverage the abstraction provides.
- Speaker notes: explain why this abstraction was worth the investment. What it unlocks.

**Slide 9: Trade-offs**
```md
---
layout: center
---
```
- **SVG**: "Trade-off matrix" -- A sketchy 2x2 or 3-column comparison: What we gained vs. What it cost vs. What we'd do differently. Use green for gains, red for costs, coral for improvements.
- **Text**: None.
- Speaker notes: be honest about the costs. Engineering credibility comes from acknowledging trade-offs.

**Slide 10: Performance / Results**
```md
---
layout: center
---
```
- **SVG**: "Metrics dashboard" -- 3-4 large stat boxes with key performance metrics. Example: "< 50ms p99 latency", "3x throughput", "1 import swap to migrate". Use large numbers in coral, labels in muted.
- **Text**: None.
- Speaker notes: explain how each metric was measured. What the baseline was.

**Slide 11: What's Next**
```md
---
layout: center
---
```
- **SVG**: "Roadmap sketch" -- A sketchy timeline with 3-4 upcoming milestones. Current position marked with a glowing dot. Future items in muted/dashed style.
- **Text**: None.
- Speaker notes: explain priorities and sequencing.

**Slide 12: Q&A**
```md
---
layout: center
---

# Questions?

<!-- Speaker notes: have architecture docs, benchmarks, and code examples ready -->
```

---

## 4. Strategy Deck

**Audience**: Business stakeholders, advisors, internal leadership.

**Story arc**: Market --> Positioning --> Revenue --> Execution Plan --> Kill Criteria.

**Length**: 14-18 slides.

**Pipeline data sources**:
- `signal-scan`: market signals, competitive landscape, demand evidence
- `decision-log`: strategic decisions, SDP scores
- `persona-extract`: target customer profiles
- `offer-scope`: revenue model, pricing, distribution
- `pitch`: positioning copy, launch plan

### Slide Sequence

**Slide 1: Cover**
```md
---
layout: cover
background: '#0d1117'
---

# PRODUCT NAME: Strategy

QUARTER/YEAR

<style>/* global styles */</style>
```

**Slide 2: Market Thesis**
```md
---
layout: center
---
```
- **SVG**: "Market shift" -- A before/after timeline showing the structural change that creates the opportunity. Left side (muted, past): how the market used to work. Right side (coral, present): what changed. Bezier arrow bridging them with the thesis statement as a label.
- **Text**: The meta-signal from signal-scan in one sentence.
- **Data source**: `signal-scan` meta-signal synthesis.
- Speaker notes: explain the structural failure that creates this opening.

**Slide 3: Market Size**
```md
---
layout: center
---
```
- **SVG**: "TAM/SAM/SOM" -- Three concentric sketchy circles (largest to smallest). Labels: Total Addressable Market (outer), Serviceable Addressable Market (middle), Serviceable Obtainable Market (inner). Numbers inside each.
- **Text**: The SOM number as a large stat.
- **Data source**: `signal-scan` DEMAND + SPEND signals for market sizing.
- Speaker notes: explain how each tier was calculated. Ground in evidence.

**Slide 4: Competitive Landscape**
```md
---
layout: center
---
```
- **SVG**: "Competitive map" -- Same 2x2 quadrant as product pitch deck slide 10, but with more detail: competitor names, their weaknesses annotated with callout lines.
- **Text**: Positioning statement.
- **Data source**: `signal-scan` COMPETITIVE signals + `decision-log` competition gap.
- Speaker notes: name each competitor's weakness. Explain why they can't or won't fix it.

**Slide 5: Target Customer**
```md
---
layout: center
---
```
- **SVG**: "Customer profile" -- Primary persona sketch with pain story headline, role, budget, decision-making authority. Simplified version of the persona card.
- **Text**: Who we sell to, in one sentence.
- **Data source**: `persona-extract` primary persona.
- Speaker notes: tell the pain story. Make the audience feel the problem.

**Slide 6: Positioning**
```md
---
layout: center
---
```
- **SVG**: "Positioning statement" -- A hand-drawn card with the positioning framework: "For [target], who [pain], [product] is a [category] that [key benefit]. Unlike [competitors], we [differentiator]." Each bracketed section in coral, the connective words in muted.
- **Text**: None -- the SVG IS the positioning statement.
- **Data source**: `pitch` positioning copy + `offer-scope` value proposition.
- Speaker notes: explain each element of the positioning. Why these specific words.

**Slide 7: Revenue Model**
```md
---
layout: center
---
```
- **SVG**: "Revenue architecture" -- A sketchy flow showing: pricing tiers (if any), revenue per customer, expected volume, total revenue. Use stacked bars or a funnel. Green for revenue numbers.
- **Text**: Key revenue metric as a large stat.
- **Data source**: `offer-scope` revenue model.
- Speaker notes: walk through the math. Explain assumptions.

**Slide 8: Distribution Strategy**
```md
---
layout: center
---
```
- **SVG**: "Distribution channels" -- 3-4 channel boxes arranged around a central product box. Each channel box labeled with the platform (Reddit, LinkedIn, email, etc.) and the strategy (value-first content, launch post, etc.). Arrows from channels to product.
- **Text**: None.
- **Data source**: `offer-scope` distribution plan + `pitch` launch posts.
- Speaker notes: explain each channel. Which is primary, which is secondary.

**Slide 9: Execution Calendar**
```md
---
layout: center
---
```
- **SVG**: "Sprint timeline" -- A sketchy Gantt-like timeline showing build phases, launch date, and post-launch milestones. Use coral for current phase, green for completed, muted for future. Include key dates.
- **Text**: Launch date as a large stat.
- **Data source**: `offer-scope` execution calendar + `pitch` launch checklist.
- Speaker notes: walk through each phase. What's the critical path.

**Slide 10: Pricing Strategy**
```md
---
layout: center
---
```
- **SVG**: "Pricing comparison" -- Side-by-side sketchy cards: our pricing vs. competitor pricing vs. what the market currently pays. Green accent on ours to show value positioning.
- **Text**: Price point and justification.
- **Data source**: `offer-scope` pricing + `signal-scan` SPEND signals.
- Speaker notes: justify the price by referencing SPEND evidence.

**Slide 11: Risk Analysis**
```md
---
layout: center
---
```
- **SVG**: "Risk matrix" -- A sketchy 2x2 grid (likelihood vs. impact). Plot 3-5 risks as labeled dots. High-likelihood + high-impact quadrant in red. Include mitigation labels as annotation callouts.
- **Text**: None.
- **Data source**: `decision-log` pre-mortem scenarios.
- Speaker notes: explain each risk and its mitigation. Be direct about what could go wrong.

**Slide 12: Kill Criteria**
```md
---
layout: center
---
```
- **SVG**: Same "Tripwire board" from validation summary deck.
- **Text**: Headline: "Exit discipline."
- **Data source**: `decision-log` + `offer-scope` kill criteria.
- Speaker notes: commit to these publicly.

**Slide 13: Success Metrics**
```md
---
layout: center
---
```
- **SVG**: "Scorecard" -- 4-5 metric boxes with: metric name, target value, measurement method, timeline. Green accent on targets.
- **Text**: None.
- **Data source**: `decision-log` success metrics + `offer-scope` kill criteria (inverted).
- Speaker notes: how and when each metric will be measured.

**Slide 14: Ask / CTA**
```md
---
layout: center
---
```
- **SVG**: Minimal -- large hand-drawn arrow or a single word.
- **Text**: What you need from the audience: decision, resources, feedback, approval.
- Speaker notes: be specific. What exactly do you need and by when.
