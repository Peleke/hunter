# Methodology Log: What We Did (For Skill Formalization)
**Date**: 2026-02-08
**Purpose**: Track the manual process so we can turn it into repeatable skills

## Pipeline Steps Executed Manually

### Step 1: Signal Scan (DONE → formalized as `signal-scan` skill)
**Input**: Domain name + operator edge profile
**Process**:
1. Parallel web research across 3 axes:
   - Product signal frameworks (what the pros track)
   - Reddit/community pain points (raw demand signals)
   - Marketplace bestseller analysis (what people actually pay for)
2. Normalize all signals into 7 canonical types (pain, demand, spend, behavior, sentiment, competitive, audience)
3. Score opportunities (pain_intensity, spend_evidence, edge_match, time_to_ship, competition_gap, audience_fit)
4. Generate 1-day ship candidates per opportunity
5. Synthesize meta-signal across domains
**Output**: Structured signal scan (JSON + markdown)
**Skill**: `signal-scan` ✅ CREATED

### Step 2: Decision Log (DONE → needs skill formalization)
**Input**: Signal scan output + operator preferences + constraints
**Process**:
1. Present opportunities ranked by score
2. Operator selects based on score + personal excitement + energy sustainability
3. Log the decision with rationale, alternatives rejected, constraints
4. Define execution order if multiple domains selected
**Output**: Decision document with rationale and next steps
**Skill**: `decision-log` — TODO

### Step 3: Deep Persona Extraction (IN PROGRESS → needs skill formalization)
**Input**: Selected opportunity + domain + pain/spend signals from scan
**Process**:
1. Deep web research for SPECIFIC stories (not general trends):
   - Pain stories: real people, real quotes, real failures
   - Success stories: what did the people who made it DO differently?
   - Emotional content: the raw human experience
2. Extract decision points (the fork in the road):
   - Triggering event (promotion, incident, new job)
   - Stuck behavior (another tutorial, copy-paste, panic)
   - Success behavior (build real things, study production, find mentor)
3. Define 3-4 persona archetypes from real stories:
   - Name, situation, emotional state
   - Willingness to pay, price sensitivity
   - Where they hang out online
   - Buying trigger (the moment they'd pay)
4. Map decision points → offers:
   - What does the person need AT THAT MOMENT?
   - What format? (PDF at 2am after incident? Video during lunch?)
   - What's the 1-day version?
   - Landing page headline (using EXACT pain language)
**Output**: Persona profiles + decision point map + offer candidates
**Skill**: `persona-extract` — TODO

### Step 4: Offer Scoping (NOT YET STARTED → needs skill formalization)
**Input**: Persona data + decision points + operator constraints
**Process**: TBD — will document after we do it manually
**Output**: 1-day build spec
**Skill**: `offer-scope` — TODO

### Step 5: Persistence (PARTIALLY DONE → needs skill formalization)
**Input**: Any pipeline output (PipelineEnvelope)
**Process**: Render to Obsidian-compatible markdown, write to vault, update kanban + session
**Output**: Obsidian files with frontmatter, tags, cross-links
**Skill**: `hunter-log` — TODO (design complete, see docs/002)

## Notes
- OpenClaw agent needs visibility into these skills (add to skill discovery path)
- All skill I/O should be JSON serializable for pipelining
- The vault path is `/Users/peleke/Documents/Vaults/ClawTheCurious/`
- Product Discovery dir already created in vault with subdirs: Signal Scans, Decisions, Personas, Offers
