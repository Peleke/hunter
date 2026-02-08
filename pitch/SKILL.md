---
name: pitch
description: Go-to-market launch engine -- takes offer-scope build spec (positioning, distribution plan, revenue model, kill criteria) plus persona data and SWOT analysis, then produces the complete launch kit: landing page copy, launch posts, GitHub product README, email sequence, launch checklist, A/B test spec, and refined kill criteria. Use when converting a validated offer spec into ready-to-execute go-to-market materials.
license: MIT
---

# Pitch

Transform an offer-scope build spec into a complete go-to-market launch kit. Produces ready-to-execute landing page copy, platform-specific launch posts, a GitHub product README (with product structure sketch for technical domains), a post-purchase email sequence, an hour-by-hour launch checklist, an A/B test spec, and refined kill criteria -- all grounded in persona data, SWOT findings, and offer-scope positioning.

## Step 0: Load Conventions

Before doing ANYTHING, read the shared conventions file:

```
Read /Users/peleke/Documents/Projects/skills/skills/custom/_conventions.md
```

This file defines: canonical vault path, folder-to-type mapping, frontmatter contract, valid statuses, tag hierarchy, cross-reference syntax, and the PipelineEnvelope schema. All output from this skill MUST conform to those conventions. If there is a conflict between this SKILL.md and `_conventions.md`, the conventions file wins.

## Pipeline Position

```
signal-scan -> decision-log -> persona-extract -> swot-analysis -> offer-scope -> **pitch** -> hunter-log
```

This skill consumes the output of `offer-scope`, `persona-extract`, `swot-analysis`, `decision-log`, and `signal-scan`, and feeds into `hunter-log` for execution tracking. The pitch output is the complete "go live" package -- everything needed to launch the product.

## When to Use

- Converting a validated offer spec into ready-to-execute launch materials
- Generating landing page copy grounded in persona language and offer-scope positioning
- Creating platform-specific launch posts that follow the 80/20 value-first rule
- Building a GitHub product README that serves as both documentation and conversion engine
- Drafting a post-purchase email sequence for nurturing and community building
- Planning a day-by-day launch execution sequence with specific times and platforms
- Defining what to test (headlines, prices, channels) once traffic is flowing
- Sharpening kill criteria inherited from offer-scope with pitch-level specifics

## Trigger Phrases

- "Build the launch kit for [product]"
- "Create pitch materials from this offer spec"
- "I need landing page copy for [product]"
- "Generate the go-to-market package"
- "Turn this offer scope into launch-ready materials"
- "/pitch [offer-scope output]"
- "Deploy artifacts from [pitch doc]"
- "Split the pitch into deployable files"
- "/pitch --deploy-only [pitch-slug or path]"

---

## Deploy-Only Mode

Reads an existing pitch document from the vault and splits/deploys its artifacts without re-generating content. Use when:
- A pitch was generated before the deployment extensions existed
- You want to re-deploy artifacts after editing the pitch document
- Testing the deployment pipeline against an existing pitch

### Deploy-Only Input

One of:
- **Pitch slug**: e.g., `devops-decision-kit-pitch-2026-02-08` (searches `Admin/Product-Discovery/Pitches/`)
- **Full vault path**: e.g., `Admin/Product-Discovery/Pitches/devops-decision-kit-pitch-2026-02-08.md`
- **Absolute path**: the full filesystem path to the pitch markdown

Optional:
- **Launch date**: ISO date for kanban task dates. If omitted, uses relative dates (`T-7`, `T+0`, `T+1`, etc.)
- **Skip list**: Deployment phases to skip (e.g., `--skip emails,kanban` if those are already deployed)

### Deploy-Only Workflow

```
Existing pitch markdown (from vault)
    |
Step 1: Parse pitch document into sections
    |
Step 2: Check what is already deployed (launchpad branch? seeds exist? kanban exists?)
    |
Step 3: Deploy missing artifacts only
    |    ├── Phase 2.4: Launch posts → Obsidian content seeds
    |    ├── Phase 4.6: Email sequence → individual files on launchpad branch
    |    ├── Phase 5.6: Launch checklist → Obsidian kanban
    |    └── Phase 7.6: Kill criteria → Obsidian Tasks on pitch doc
    |
Step 4: Update Pipeline tracker
    |
Output: Report of what was deployed and where
```

### Parsing Contract

The deploy-only mode reads the pitch markdown and locates content by H2 section headers. The pitch doc may use either short headers (`## Launch Posts`) or phase-numbered headers (`## Phase 2: Launch Posts`). Match on the section name, ignoring any `Phase N:` prefix.

| Pitch Section | Header (either format) | Parser | Deploy Phase |
|---|---|---|---|
| Launch Posts | `## Launch Posts` or `## Phase 2: Launch Posts` | Split by `### {Platform}: {Title}`, `### Reddit r/{sub} Post`, or `### Twitter/X Thread` | 2.4 (Obsidian seeds) |
| Email Sequence | `## Email Sequence` or `## Phase 4: Email Sequence` | Split by `### Email {N}: {Subject} (Day {N})` | 4.6 (individual files) |
| Launch Checklist | `## Launch Checklist` or `## Phase 5: Launch Checklist` | Split by `### Pre-Launch`, `### Launch Day`, `### Week 1`, `### Ongoing Cadence`, `### Month 2-3 Growth` | 5.6 (Obsidian kanban) |
| Kill Criteria | `## Kill Criteria` or `## Phase 7: Kill Criteria` | Read `### 7.1 Week 1 Threshold`, `### 7.2 Month 1 Threshold`, `### 7.3 Month 3 Threshold` (also accepts unnumbered `### Week 1`, etc.) | 7.6 (Obsidian Tasks) |

**If a section is missing or cannot be parsed, skip that deployment phase and warn the operator.** Do not fail the entire deploy because one section is malformed.

### Deploy-Only Checks

Before deploying each artifact:

1. **Content seeds** (Phase 2.4): Check if `{vault}/Writing/Content-Briefs/{product-slug}-{platform}-seed-*.md` already exists. If yes, skip and warn (or append under `## Update: {timestamp}` per conventions).
2. **Email files** (Phase 4.6): Check if `emails/` directory exists on the launchpad branch. If yes, skip and warn. If the branch does not exist, warn that Phase 3d (launchpad branch creation) should run first.
3. **Kanban** (Phase 5.6): Check if `{vault}/Admin/Product-Discovery/Pitches/{product-slug}-launch-checklist.kanban.md` exists. If yes, skip and warn.
4. **Review reminders** (Phase 7.6): Check if the pitch markdown already has a `## Review Reminders` section. If yes, skip and warn.

### Deploy-Only Output

Returns a deployment report:

```json
{
  "pitch_ref": "devops-decision-kit-pitch-2026-02-08",
  "deployed": {
    "content_seeds": ["path/to/seed-1.md", "path/to/seed-2.md"],
    "email_files": ["emails/email-1-welcome.md", ...],
    "kanban": "path/to/launch-checklist.kanban.md",
    "review_reminders": "appended to pitch markdown"
  },
  "skipped": {
    "kanban": "already exists at path/to/..."
  },
  "warnings": ["No launch date specified; using relative dates"]
}
```

---

## Prerequisites

Before starting, the following must be available:

1. **Offer-scope output** -- A completed offer-scope result containing: product name, format, price, value equation, build spec, positioning (headline, subheadline, bullet points, objection handlers, social proof angle, guarantee), distribution plan, revenue model, and kill criteria
2. **Persona extraction output** -- The persona used by offer-scope, containing: pain stories (with situation/pain/workaround/emotional state/evidence), decision triggers, objections, willingness-to-pay data, and channel behavior
3. **SWOT analysis output** -- The SWOT for this hypothesis, containing: validated strengths, key weaknesses, opportunities, threats, risk registry, and moat assessment
4. **Decision log entry** -- The decision record for the chosen opportunity
5. **Signal scan data** -- The original signal scan with PAIN, SPEND, COMPETITIVE, and AUDIENCE signals

If any of these are missing, prompt the user to run the upstream skill first or provide the data manually. The pitch skill is a synthesis layer -- it assembles and refines upstream outputs into execution-ready materials, not a research layer.

---

## Input

The skill expects the following input structure (assembled from upstream skill outputs):

```typescript
interface PitchInput {
  offer: {
    product_name: string
    format: string
    price_point: string
    ship_time: string
    value_equation: {
      dream_outcome: string
      perceived_likelihood: string
      time_delay: string
      effort_sacrifice: string
    }
    build_spec: {
      deliverable: string
      transformation: string
      sections: { title: string, estimated_time: string, content_type: string }[]
    }
    positioning: {
      headline: string
      subheadline: string
      bullet_points: string[]
      objection_handlers: { objection: string, counter: string }[]
      social_proof_angle: string
      guarantee: string
    }
    distribution_plan: {
      launch_channel: string
      launch_strategy: string
      ongoing_channels: string[]
      email_capture: boolean
    }
    revenue_model: {
      unit_price: number
      first_week_target: { units: number, revenue: number }
      first_month_target: { units: number, revenue: number }
      assumptions: string[]
    }
    kill_criteria: string[]
  }
  persona: {
    persona_name: string
    pain_stories: {
      situation: string
      pain: string
      current_workaround: string
      emotional_state: string
      evidence: string
    }[]
    decision_triggers: { trigger: string, urgency: string, channel: string }[]
    objections: { objection: string, counter: string }[]
    willingness_to_pay: { range: string, evidence: string, anchor_products: string[] }
    channels: { platform: string, behavior: string }[]
  }
  swot: {
    verdict: string
    validated_strengths: string[]
    key_risks: string[]
    modifications: string[]
    moat_assessment: { current_strength: string, timeline: object }
  }
  domain: string
  opportunity: string
  offer_ref: string
  persona_ref: string
  swot_ref: string
  decision_ref: string
  signal_scan_ref: string
}
```

---

## Workflow

```
Offer Spec + Persona + SWOT + Decision + Signal Scan (from upstream)
    |
Phase 1: Landing Page Copy (complete, ready to paste)
    |
Phase 2: Launch Posts (platform-specific, ready to publish)
    |
Phase 3: GitHub Product README (Phase 3a: product structure + Phase 3b: README copy)
    |
Phase 4: Email Sequence (3-5 emails, post-purchase nurture)
    |
Phase 5: Launch Checklist (hour-by-hour execution plan)
    |
Phase 6: A/B Test Spec (what to test once traffic flows)
    |
Phase 7: Kill Criteria Refinement (time-bounded, measurable)
    |
Output: JSON spec + Markdown summary -> vault
```

---

## Phase 1: Landing Page Copy

Generate complete landing page copy that is ready to paste into any landing page builder (Carrd, Webflow, Framer, raw HTML). Every section must be standalone -- someone could build a full conversion page from this output alone.

### 1.1 Headline + Subheadline

Refine the headline and subheadline from offer-scope positioning. The headline from offer-scope is a draft; the pitch headline is the final version.

- **Headline**: Must stop the scroll. Pull directly from the persona's pain language. Test: if this headline appeared in the subreddit where the persona hangs out, would they stop scrolling and click?
- **Subheadline**: States the transformation in concrete terms -- what they get, what changes, how fast. No vague promises. Specificity is credibility.

### 1.2 Above-the-Fold Hook

The first 2-3 sentences below the headline. This is the "stay or bounce" copy. It must:
- Acknowledge the persona's current painful situation (using their exact words from pain stories)
- Promise a specific, believable outcome
- Create enough curiosity to scroll

### 1.3 Problem Section

Full copy for the "you know this feeling" section. Structure:
- **Situation**: What the persona is doing when the pain hits (from pain stories)
- **Pain**: What goes wrong, how it feels (using emotional state from persona data)
- **Failed workarounds**: What they have tried that does not work (from current_workaround in pain stories)
- **Emotional climax**: The moment of peak frustration -- this is where the persona thinks "this person has been in my shoes"

Rules:
- Use direct quotes or near-quotes from persona evidence
- Do NOT use generic pain language ("Are you tired of...?"). Use specific language ("You are staring at a Terraform plan that touches 47 resources and you have no idea which ones are safe to apply")
- Minimum 3 pain story references, cited from persona data

### 1.4 Solution Section

Full copy for "here is what changes." Structure:
- **The transformation**: Before state to after state (from offer-scope transformation)
- **How it works**: 3-step simplified explanation (not the full build spec, but the buyer's journey)
- **Speed**: How fast they see results (from value equation time_delay)
- **Effort required**: How little work they do (from value equation effort_sacrifice)

### 1.5 What's Inside Section

Section-by-section breakdown from the offer-scope build spec. For each section:
- **Section name**: From build spec section titles
- **What it does for them**: Translate the content_type into a buyer benefit
- **Key deliverable**: The specific thing they walk away with from this section

Rules:
- Use benefit language, not feature language ("A decision tree that tells you exactly which container orchestrator to use" not "Container orchestration decision tree")
- Each section description should make the buyer think "I need that specific thing"

### 1.6 Social Proof Strategy

For a zero-testimonial launch, build credibility through:
- **Credentials proof**: Operator's relevant experience, deployed to match persona's authority bias
- **Process proof**: How this was built -- research methodology, persona data, real-world grounding
- **Methodology proof**: The frameworks and thinking behind the product (named frameworks from the canon)

Do NOT fabricate testimonials. Do NOT use placeholder quotes. Honest credibility beats fake social proof.

### 1.7 Pricing Section

Full copy including:
- **Price anchor**: Show the value first (what this would cost as consulting, what comparable resources cost)
- **Value stack**: List everything they get with dollar values assigned to each component
- **The price**: Clear, prominent, no hiding
- **Guarantee**: From offer-scope, written as full copy (not just the policy, but the emotional reassurance)
- **Urgency element**: If applicable (launch pricing, early-bird, limited bonus) -- only if honest

### 1.8 FAQ Section

Built from persona objections. Each FAQ maps to one objection from persona data:
- **Question**: The objection reframed as a question the buyer would actually ask
- **Answer**: The counter from offer-scope objection handlers, expanded into conversational copy

Minimum 5 FAQs. Each must trace to a specific persona objection.

### 1.9 CTA Copy

- **Primary CTA**: The main action (buy, enroll, download). Button text + surrounding copy. Must be specific ("Get the DevOps Decision Kit" not "Buy Now")
- **Secondary CTA**: The lower-commitment action (email signup, free preview, GitHub repo). For people who are not ready to buy yet but are interested.

### Landing Page Copy Rules

- Every line must be traceable to persona data or offer-scope positioning. If you cannot cite the source, cut the line.
- No generic marketing fluff. No "In today's fast-paced world..." No "Are you ready to take your career to the next level?"
- The landing page must work as a standalone document -- someone could build a page from just this output.
- Write in the persona's register. If the persona is a senior engineer, write like a senior engineer talks, not like a marketer talks.

---

## Phase 2: Launch Posts

Generate 2-3 platform-specific posts, ready to publish. Each follows the 80/20 value-first rule: give 80% of the core insight for free in the post itself, sell the organized/actionable version.

### 2.1 Primary Channel Post

From offer-scope distribution plan, identify the primary launch channel (e.g., r/devops, dev.to, LinkedIn). Generate the full post:

- **Title/Hook**: Platform-appropriate. Reddit titles are direct and specific. LinkedIn hooks are pattern-interrupts. dev.to titles are tutorial-style.
- **Body**: The core insight, framework, or decision logic from the product -- presented as a genuine, valuable standalone post. The reader should benefit even if they never click the link.
- **CTA placement**: Natural transition from "here is the free insight" to "here is the organized/complete version." Never bait-and-switch.
- **Engagement hooks**: Questions, calls for discussion, requests for feedback -- things that drive comments and shares.
- **Formatting notes**: Platform-specific formatting (Reddit markdown, paragraph breaks, header usage).

### 2.2 LinkedIn Version

LinkedIn-specific formatting and tone:
- Opening hook (first 2 lines are visible before "see more" -- must be compelling)
- Short paragraphs (1-2 sentences each)
- Story arc: problem -> insight -> framework -> result -> CTA
- Professional but not corporate. Practitioner voice, not thought-leader voice.
- Hashtag strategy (3-5 relevant hashtags)

### 2.3 Twitter/X Thread Version

5-8 tweet thread:
- Tweet 1: The hook -- a bold, specific claim or observation. Must stand alone as engaging content.
- Tweets 2-5: The framework/insight. Each tweet is self-contained and valuable. Use numbered lists, examples, and specific details.
- Tweet 6-7: The personal experience or credibility signal. Why you built this.
- Final tweet: The CTA. Link to product or landing page. Retweet request.
- Thread pacing: alternate between insight tweets and example tweets. Do not front-load all the value or back-load all the value.

### Launch Post Rules

- Posts must provide GENUINE value. Someone who never buys should still benefit from reading the post.
- The CTA should feel like a natural next step, not a sales pitch grafted onto content.
- Follow platform norms: no self-promo spam on Reddit (provide value first, link second), no clickbait on LinkedIn (be direct), no engagement-bait on Twitter (be authentic).
- Each post must be ready to publish. Not an outline, not a draft. The final words.

### 2.4 Obsidian Seed Output

After generating launch posts, write each post as a **content seed** to the Obsidian vault. These are NOT finished briefs -- they are raw seeds for the operator to review and refine. The `content-planner` skill picks these up later for full brief generation.

Write each post to: `{vault}/Writing/Content-Briefs/{product-slug}-{platform}-seed-{YYYY-MM-DD}.md`

Frontmatter:
```yaml
---
type: content-brief
date: YYYY-MM-DD
status: backlog
tags:
  - content/brief
  - content/channel/{platform}
  - hunter/domain/{domain-slug}
source_skill: pitch
pitch_ref: "{pitch-slug}"
platform: reddit | linkedin | twitter
---
```

Body: The full post text from Phase 2, preceded by a `## Seed Context` section with:
- Target platform and subreddit/hashtags
- Persona being addressed
- Key offer-scope positioning points used
- Suggested posting time (from launch checklist if available)

These seeds flow into the content-planner pipeline: seed → operator review (via PR) → content-planner generates full brief → schedule → publish.

---

## Phase 3: GitHub Product README

This phase has two sub-phases. Phase 3a produces the product architecture; Phase 3b produces the README that serves as both documentation and conversion engine.

### Phase 3a: Product Structure Sketch

**ACTIVATES for technical domains**: DevOps, security, coding, architecture, STEM, data engineering, ML/AI, infrastructure. Skip for non-technical domains (business coaching, creative writing, etc.).

This sub-phase produces the actual PRODUCT DESIGN for the GitHub repo. It is the blueprint for what the repo contains and how it is structured.

#### Buildlog Integration (Ambient Data Enrichment)

Before sketching the structure, check buildlog for real engineering material to instrument the product with:

1. **Scan buildlog entries**: Use `buildlog_entry_list()` and `buildlog_overview()` to find entries relevant to the product domain
2. **Pull decision narratives**: Extract "we chose X over Y because Z" stories from buildlog entries and reward events -- these become case studies and decision annotations in the product
3. **Harvest real code examples**: Find real build errors, debugging sessions, and solutions from buildlog -- these replace hypothetical examples with authentic, battle-tested ones
4. **Collect learnings and mistakes**: Real "what went wrong" stories from buildlog entries are more valuable than invented scenarios
5. **Reference specific entries**: In the file manifest, note which buildlog entries source each example (e.g., `buildlog://hunter/terraform-ecs-vs-eks-decision`)

The goal: every example, narrative, and code snippet in the product should come from REAL engineering work captured by buildlog, not from hypothetical scenarios. This is the operator's unfair advantage -- authentic practitioner content that "course creators" cannot replicate.

#### Directory Layout

Generate the actual directory tree:
```
repo-name/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── docs/
│   ├── getting-started.md
│   └── ...
├── frameworks/
│   ├── ...
│   └── ...
├── checklists/
│   ├── ...
│   └── ...
├── templates/
│   ├── ...
│   └── ...
└── examples/
    ├── ...
    └── ...
```

The directory layout must reflect the build spec sections from offer-scope. Each section maps to a directory or file.

#### File Manifest

Key files with one-line descriptions:
- List every file that will exist in the repo
- Each file gets a one-line description of what it contains and why it matters
- Include both free-tier files (the 80% value) and premium-tier files (the 20% that people pay for)

#### Learning Path

How someone progresses through the repo:
- Step 1: Start Here (README orientation)
- Step 2: [First framework/tool]
- Step 3: [Second framework/tool]
- ...
- Step N: Get the complete kit (CTA for paid product)

#### Prerequisites

What the user needs to know before using this repo:
- Technical prerequisites (tools, languages, concepts)
- Experience level assumed
- Time commitment expected

### Phase 3b: README Conversion Copy

Generate the full README.md content that combines the product structure with conversion-optimized copy.

#### Hero Section
- What this is (one sentence)
- Why it matters (the pain it solves, using persona language)
- The transformation (before/after)
- Badges: stars, license, last commit -- the trust signals for technical audiences

#### Product Structure Preview
- The directory tree from Phase 3a
- Brief description of what each top-level directory contains
- "What You Get" summary

#### Decision Framework Preview
- The free 80%. Include enough of the actual framework, decision tree, or checklist to be genuinely useful.
- This is NOT a teaser. This is real, usable content. A developer who reads only the README should learn something valuable.
- Format: tables, decision trees (text-based), checklists, or annotated examples -- whatever matches the product format

#### "Get the Full Kit" CTA Section
- What the paid product includes beyond the free preview
- Price and link
- Guarantee
- Keep it brief -- 3-5 lines. The README proved the value; the CTA collects the conversion.

#### Email Signup CTA
- For the lead magnet funnel
- What they get for signing up (specific deliverable, not "updates")
- Link to signup form

#### Social Proof / Credentials Section
- Operator background relevant to this product
- Methodology description
- Any metrics (stars, forks, community size) once available

#### Contributing Section
- How to contribute (report issues, suggest frameworks, submit case studies)
- Code of conduct reference
- This section encourages organic growth through community participation

#### License Section
- License type and brief explanation

### GitHub README Rules

- The README must be excellent technical documentation FIRST, marketing second.
- DevOps engineers (and engineers generally) judge README quality as a proxy for product quality. Sloppy README = sloppy product.
- Code examples, directory trees, and technical precision matter more than sales language.
- The free preview must be genuinely valuable. This is the "give 80%" principle in action.
- Markdown formatting must be flawless. Broken tables, inconsistent headers, or missing code fences will cost credibility.

### Phase 3c: Hero Image Generation

After creating the README, generate a hero image for the product repo.

**If ComfyUI MCP is available:**
1. Use `mcp__comfyui__generate_image` to generate a product-appropriate hero image
2. Prompt should match the product domain: for DevOps/infrastructure, use isometric technical illustrations with dark backgrounds and teal/blue accents; for programming, use code-themed abstract art; for security, use shield/lock motifs
3. Dimensions: 1280x640 (GitHub README hero ratio)
4. Save to the product branch root as `hero.png`
5. Reference in README: `![{Product Name}](hero.png)` immediately after the H1 title

**If ComfyUI MCP is NOT available:**
Insert an HTML comment in the README immediately after the H1 title with the image intent:
```html
<!-- IMAGE_INTENT: {describe the ideal hero image -- style, colors, elements, mood}
     PROMPT: {the exact ComfyUI prompt to use when generation becomes available}
     DIMENSIONS: 1280x640 -->
```

This allows a future run (or a human designer) to generate the image from the intent.

### Phase 3d: Launchpad Branch + PR

After generating the README and product structure, deploy to the `launchpad` repo:

1. **Clone `launchpad`**: `git clone https://github.com/Peleke/launchpad.git`
2. **Create product branch**: `git checkout -b {product-slug}`
3. **Scaffold the directory structure** from Phase 3a: create all directories and placeholder READMEs
4. **Write the full README.md** from Phase 3b
5. **Add hero image** from Phase 3c (or the HTML comment fallback)
6. **Add scaffolding files**: LICENSE, CONTRIBUTING.md, issue templates
7. **Commit and push** the branch
8. **Open a PR against main** using `gh pr create`:
   - Title: `{Product Name}: Product Structure + README + Hero`
   - Body: Product summary, file list, review checklist with checkboxes, pipeline refs (offer_ref, persona_ref, swot_ref)
   - The PR is the operator's review surface for line-by-line feedback

The PR stays open until the operator reviews and merges. The operator can comment on specific files, and the agent can address feedback in subsequent commits to the same branch.

---

## Phase 4: Email Sequence

Generate 3-5 emails for the post-purchase nurture sequence. These build trust, deliver value, and create the path to community.

### Email 1: Welcome / Delivery (Send: Immediately)

- **Subject line**: Specific to what they just bought. Not "Welcome!" but "Your [Product Name] is ready -- here is where to start"
- **Purpose**: Deliver the product, reduce buyer's remorse, create immediate momentum
- **Body**:
  - Thank them (brief, genuine, not gushing)
  - Delivery link/instructions
  - Quick Start: "Open [specific file/section] first. It will take you 10 minutes and you will have [specific outcome]."
  - What to expect from future emails (set expectations)
- **Single job**: Get them to open the product and complete one action

### Email 2: Quick Win (Send: Day 2)

- **Subject line**: Specific action they can take right now. Not "Tips for success" but "Try this with your next [specific task] -- takes 5 minutes"
- **Purpose**: Help them use the product and get a tangible result
- **Body**:
  - One specific technique or framework from the product
  - Step-by-step instructions (3-5 steps)
  - Expected outcome: "After doing this, you will have [specific result]"
  - Reply hook: "Hit reply and tell me how it went"
- **Single job**: Get them a concrete win that validates the purchase

### Email 3: Story / Credibility (Send: Day 4)

- **Subject line**: A specific production war story. Not "My journey" but "The deploy that taught me [specific lesson]"
- **Purpose**: Build trust through shared experience. Establish that the operator has been where the buyer is.
- **Body**:
  - A real story from the operator's experience (specific situation, specific mistake, specific lesson)
  - How this experience shaped the product they just bought
  - Connection to a specific part of the product: "This is why Section [X] exists"
- **Single job**: Make the buyer think "this person has earned the right to teach me this"

### Email 4: Objection Handler (Send: Day 6)

- **Subject line**: Address the top objection directly. Not "Common questions" but "You might be wondering if [specific concern]"
- **Purpose**: Address the #1 post-purchase doubt from persona objection data
- **Body**:
  - Acknowledge the concern honestly (from persona objections)
  - Counter with evidence (from offer-scope objection handlers)
  - Proof point: a specific example, case study, or data point
  - Bridge to action: "Here is exactly how to [specific next step]"
- **Single job**: Eliminate the biggest doubt that prevents them from using the product fully

### Email 5: Community Invite (Send: Day 7)

- **Subject line**: Invitation, not pitch. "Where [persona type] share real [domain] decisions"
- **Purpose**: Soft pitch for the Skool community (if/when it exists). If no community yet, this becomes a feedback request.
- **Body**:
  - Transition: "You have the framework. Now the question is: where do you practice?"
  - Community description: what it is, who is in it, what happens there
  - Specific value: "Last week, someone posted [specific example of community value]"
  - Link to join
  - If no community yet: "I am building a community for [persona type]. Reply if you would want in -- I am looking for founding members."
- **Single job**: Convert a customer into a community member (or collect intent data for community launch)

### Email Sequence Rules

- Subject lines must be specific. Not "Hey!" or "Quick update." Specific enough that the persona knows exactly what the email is about before opening.
- Each email has ONE job. Do not cram multiple CTAs. Do not ask them to buy something AND join a community AND share on social media.
- Tone: practitioner-to-practitioner, not marketer-to-mark. You are someone who builds production systems writing to someone else who builds production systems.
- No fake urgency. No countdown timers. No "this offer expires." The product is good. The emails prove it. That is the selling.

### 4.6 Email File Output

After generating the full email sequence, split each email into its own file on the launchpad product branch:

```
{product-branch}/
└── emails/
    ├── README.md              # Sequence overview: timing, purpose of each email
    ├── email-1-welcome.md     # Day 0: Welcome / Delivery
    ├── email-2-quick-win.md   # Day 2: Quick Win
    ├── email-3-story.md       # Day 4: Story / Credibility
    ├── email-4-objection.md   # Day 6: Objection Handler
    └── email-5-community.md   # Day 7: Community Invite
```

Each email file contains:
- Subject line
- Full body copy (ready to paste into ConvertKit/Buttondown)
- Send timing (day + recommended time)
- Single job description (what this email is supposed to accomplish)

The `emails/README.md` contains the sequence overview: timing map, purpose of each email, and notes on tone/voice for anyone editing later.

---

## Phase 5: Launch Checklist

Step-by-step execution sequence with specific dates, times, and platforms. This is the "do this, then do this" plan for going from "product is built" to "product is selling."

### 5.1 Pre-Launch Setup (Days -7 to -1)

For each task:
- **Task**: What to do
- **Tool needed**: Specific tool (Gumroad, LemonSqueezy, ConvertKit, Buttondown, GitHub, Carrd, etc.)
- **Estimated time**: How long this takes

Minimum tasks:
- Set up payment processing (Gumroad/LemonSqueezy product page)
- Set up email tool (ConvertKit/Buttondown) and import email sequence
- Create landing page (paste Phase 1 copy into builder)
- Set up GitHub repo (paste Phase 3 README, create directory structure)
- Prepare all launch posts (Phase 2 posts finalized and saved as drafts)
- Test the full funnel: landing page -> payment -> delivery -> email 1
- Create a lead magnet landing page (if email capture is in the distribution plan)

### 5.2 Launch Day (Day 0)

Hour-by-hour sequence:
- **Time**: Specific time (e.g., "9:00 AM EST")
- **Action**: Specific action (e.g., "Publish Reddit post to r/devops")
- **Platform**: Where

Example structure:
- 8:00 AM: Final check -- all links work, payment processes, emails send
- 9:00 AM: Publish primary channel post (Reddit/dev.to)
- 10:00 AM: Publish LinkedIn post
- 11:00 AM: Publish Twitter/X thread
- 12:00 PM: Check for comments on primary post, engage with every comment
- 2:00 PM: Share in any relevant Slack/Discord communities
- 4:00 PM: First engagement check -- respond to all comments and DMs
- 8:00 PM: End-of-day engagement pass -- respond to everything, note feedback themes

### 5.3 Week 1 Follow-Up (Days 1-7)

Daily actions:
- **Day**: Day number
- **Actions**: List of specific actions

Minimum:
- Day 1: Engage with all comments from launch. Note which platforms got traction. Send thank-you DMs to anyone who shared.
- Day 2: Post a follow-up comment or update on the primary channel (not a new post -- update the existing conversation).
- Day 3: Cross-post to a secondary channel if the primary channel showed traction.
- Day 4: Write and publish a supporting content piece (a shorter version of the launch post insight, for a different angle).
- Day 5: Review sales data. How many units? From which channels? Which post drove traffic?
- Day 6: Iterate on what is working -- double down on the channel that converted, pause the channel that did not.
- Day 7: Week 1 retrospective. Compare against kill criteria. Decision point: continue, iterate, or kill.

### 5.4 Ongoing Cadence (Weeks 2-4)

- **Frequency**: How often (daily, 2x/week, weekly)
- **Action**: What to publish or do
- **Platform**: Where

Minimum:
- Publish 1-2 value-first posts per week on the primary channel
- Engage in community discussions (not self-promotion -- genuine participation)
- Send weekly email to list (if email capture is active)
- Update GitHub repo with improvements based on feedback
- Track metrics weekly: traffic, conversion rate, email signups

### 5.5 Month 2-3 Growth Actions

Based on what is working:
- If Reddit is converting: increase posting cadence, identify sub-communities
- If email is converting: optimize signup funnel, create additional lead magnets
- If GitHub is driving traffic: invest in README improvements, create issues for community contribution
- If nothing is converting: revisit kill criteria. Is it time to pivot?

### 5.6 Obsidian Kanban Output

After generating the launch checklist, write it as an Obsidian Kanban board to the vault:

**Path:** `{vault}/Admin/Product-Discovery/Pitches/{product-slug}-launch-checklist.kanban.md`

Use the Obsidian Kanban plugin format:

```markdown
---
type: kanban
date: YYYY-MM-DD
tags:
  - hunter/pitch
  - hunter/domain/{domain-slug}
pitch_ref: "{pitch-slug}"
kanban-plugin: basic
---

## Pre-Launch (Days -7 to -1)

- [ ] Set up payment processing (Gumroad/LemonSqueezy) @{date}
- [ ] Set up email tool (ConvertKit/Buttondown) @{date}
- [ ] Create landing page @{date}
- [ ] Set up GitHub repo from launchpad branch @{date}
- [ ] Prepare all launch posts as drafts @{date}
- [ ] Test full funnel: landing page → payment → delivery → email 1 @{date}

## Launch Day

- [ ] 8 AM: Final checks -- all links work @{launch_date}
- [ ] 9 AM: Publish primary channel post @{launch_date}
- [ ] 10 AM: Publish LinkedIn post @{launch_date}
- [ ] 11 AM: Publish Twitter/X thread @{launch_date}
- [ ] 12 PM: First engagement check @{launch_date}
- [ ] 4 PM: Second engagement check @{launch_date}
- [ ] 8 PM: End-of-day engagement pass @{launch_date}

## Week 1

- [ ] Day 1: Engage all comments, send thank-you DMs @{date+1}
- [ ] Day 2: Post follow-up comment on primary channel @{date+2}
- [ ] Day 3: Cross-post to secondary channel @{date+3}
- [ ] Day 4: Publish supporting content piece @{date+4}
- [ ] Day 5: Review sales data @{date+5}
- [ ] Day 6: Double down on working channel @{date+6}
- [ ] Day 7: Week 1 retrospective vs kill criteria @{date+7}

## Ongoing (Weeks 2-4)

- [ ] 2x/week value-first posts on primary channel
- [ ] 3x/week LinkedIn short-form posts
- [ ] 1x/week DEV.to or blog article
- [ ] Daily community engagement (15 min)
- [ ] Weekly metrics check (Friday)
- [ ] Weekly GitHub repo update

## Complete

```

Replace `@{date}` placeholders with actual dates based on the operator's target launch date. If no launch date is specified, use relative dates (e.g., `@{T-7}`, `@{T-0}`, `@{T+1}`).

---

## Phase 6: A/B Test Spec

Define what to test once distribution is flowing. This is NOT enterprise CRO. This is what a solo operator can actually test with 100-500 visitors.

### 6.1 Headline Variants

Generate 2-3 headline alternatives with rationale:
- **Variant A**: [headline] -- Rationale: [why this might work]
- **Variant B**: [headline] -- Rationale: [why this might work]
- **Variant C**: [headline] -- Rationale: [why this might work]

Each variant should take a different angle: pain-focused, outcome-focused, curiosity-focused, specificity-focused.

### 6.2 Price Point Variants

Generate 2-3 price alternatives:
- **Variant A**: $[price] -- Rationale: [positioning at this price, what it signals]
- **Variant B**: $[price] -- Rationale: [positioning at this price, what it signals]
- **Variant C**: $[price] -- Rationale: [positioning at this price, what it signals]

Cross-reference with offer-scope SPEND data and format ceiling.

### 6.3 Channel Priority Order

Rank channels by expected ROI:
- **Channel 1**: [channel] -- Rationale: [why test first] -- Test budget: [time/money]
- **Channel 2**: [channel] -- Rationale: [why second] -- Test budget: [time/money]
- **Channel 3**: [channel] -- Rationale: [why third] -- Test budget: [time/money]

### 6.4 Metrics to Track

For each metric:
- **Metric**: What to measure
- **Target**: What "good" looks like
- **Measurement method**: Specific tool or method (Gumroad analytics, Google Analytics, UTM parameters, manual tracking)

Minimum metrics:
- Landing page visitors (by source)
- Landing page conversion rate (visitors -> purchase)
- Email signup rate (visitors -> email)
- Launch post engagement (upvotes, comments, shares by platform)
- Revenue (total, by channel)

### 6.5 Decision Thresholds

For each test:
- **Metric**: What you are measuring
- **Threshold**: At what point do you declare a winner
- **Action**: What you do when the threshold is hit

Example: "If Headline B gets 2x the click-through rate of Headline A over 200+ visitors, switch to Headline B permanently."

### A/B Test Rules

- Solo operators cannot run statistically significant A/B tests with 50 visitors. Use sequential testing: try one version for a week, measure, try another, compare. Not perfect, but actionable.
- Test one variable at a time. Do not change the headline AND the price AND the CTA simultaneously.
- Define the decision threshold BEFORE running the test. Otherwise you will rationalize any result.

---

## Phase 7: Kill Criteria Refinement

Inherit kill criteria from offer-scope and SWOT risk registry. Sharpen them with pitch-level specifics -- exact numbers, exact dates, exact actions.

### 7.1 Week 1 Threshold

- **Metric**: What to measure (e.g., units sold, launch post engagement, email signups)
- **Threshold**: Minimum acceptable number (e.g., "5 units sold" or "launch post reaches 50+ upvotes")
- **Action if below threshold**: What to do (iterate on positioning, try a different channel, or kill)

### 7.2 Month 1 Threshold

- **Metric**: Cumulative measure (e.g., total units, total revenue, email list size, repo stars)
- **Threshold**: Minimum acceptable number
- **Action if below threshold**: Specific pivot or kill action

### 7.3 Month 3 Threshold

- **Metric**: Growth trajectory measure (e.g., MRR if community launched, month-over-month growth rate, organic traffic trend)
- **Threshold**: Minimum acceptable trajectory
- **Action if below threshold**: Strategic decision -- pivot product, pivot audience, or full kill

### 7.4 Qualitative Signals

Signals that do not have clean numbers but indicate viability:
- What kind of feedback means "iterate" (e.g., "people engage with the free content but do not convert" = pricing or value perception issue)
- What kind of feedback means "pivot" (e.g., "people love the concept but want it in a different format" = format mismatch)
- What kind of feedback means "kill" (e.g., "zero engagement on free content across all channels" = positioning failure or wrong audience)

### 7.5 Pivot Triggers

Specific conditions that trigger a strategy change vs a full kill:
- **Trigger**: [specific condition]
- **Pivot direction**: [what changes -- audience, format, price, channel, or positioning]
- **Kill condition**: [when a pivot is not enough -- the hypothesis itself is wrong]

### Kill Criteria Rules

- Every threshold must be time-bounded and measurable. Not "get some traction" but "sell 10 units in the first 14 days."
- Kill criteria must be written BEFORE launch and agreed to BEFORE emotional investment builds.
- Distinguish between "iterate" (the mechanism needs tuning), "pivot" (the approach needs changing), and "kill" (the opportunity is not viable).

### 7.6 Obsidian Task Output

After generating kill criteria, write review reminder tasks to the pitch vault document using Obsidian Tasks plugin format. Append these to the pitch markdown under a `## Review Reminders` section:

```markdown
## Review Reminders

> [!warning] These are pre-committed review dates. Check the numbers. Do not rationalize.

- [ ] **Week 1 review**: Check units sold >= {week1_threshold}, launch post engagement, landing page visitors 📅 {launch_date + 7} #hunter/review
- [ ] **Month 1 review**: Check cumulative units >= {month1_threshold}, revenue >= ${month1_revenue}, email list >= {email_threshold}, GitHub stars >= {stars_threshold} 📅 {launch_date + 30} #hunter/review
- [ ] **Month 3 review**: Check growth trajectory (MoM positive?), cumulative units >= {month3_threshold}, community viability 📅 {launch_date + 90} #hunter/review
- [ ] **Time investment audit**: Total hours invested post-build. If > 40 hours and < 10 sales, trigger kill criteria review 📅 {launch_date + 30} #hunter/review
```

Replace all `{placeholder}` values with the actual numbers from Phase 7 thresholds. The `📅` date and `#hunter/review` tag make these queryable by Obsidian Tasks and Dataview.

If the operator has not specified a launch date, use `📅 TBD` and note that dates should be filled in when a launch date is set.

---

## Output

The pitch produces multiple output artifacts across the vault and launchpad repo:

**Vault path:** `/Users/peleke/Library/Mobile Documents/iCloud~md~obsidian/Documents/ClawTheCurious/`

### Primary Outputs (Vault)

#### 1. JSON Spec: `{vault}/Admin/Product-Discovery/Pitches/{product-slug}-pitch-{YYYY-MM-DD}.json`

Structured data following the schema in [references/output-schema.json](references/output-schema.json). Must validate against that schema.

#### 2. Markdown Summary: `{vault}/Admin/Product-Discovery/Pitches/{product-slug}-pitch-{YYYY-MM-DD}.md`

Human-readable launch kit with the following structure:

```markdown
---
type: pitch
date: YYYY-MM-DD
status: draft
tags:
  - hunter/pitch
  - hunter/domain/{domain-slug}
  - hunter/opportunity/{opportunity-slug}
offer_ref: "{offer-slug}"
persona_ref: "{persona-slug}"
swot_ref: "{swot-slug}"
decision_ref: "{decision-slug}"
signal_scan_ref: "{signal-scan-slug}"
---

# Pitch: [Product Name]

**Date**: [date]
**Domain**: [domain]
**Opportunity**: [opportunity]
**Product**: [product name]
**Format**: [format]
**Price**: [price]

---

## Landing Page Copy

### Headline
[headline]

### Subheadline
[subheadline]

### Above the Fold
[hook copy]

### The Problem
[full problem section copy]

### The Solution
[full solution section copy]

### What's Inside
[section-by-section breakdown]

### Social Proof
[social proof strategy copy]

### Pricing
[full pricing section copy]

### FAQ
[all FAQs]

### Call to Action
**Primary**: [CTA copy]
**Secondary**: [CTA copy]

---

## Launch Posts

### [Platform 1]: [Title]
[full post]

### [Platform 2]: [Title]
[full post]

### Twitter/X Thread
[full thread]

---

## GitHub README

### Product Structure (if technical domain)
[directory layout, file manifest, learning path, prerequisites]

### README.md Content
[full README content]

---

## Email Sequence

### Email 1: [Subject Line] (Day 0)
[full email]

### Email 2: [Subject Line] (Day 2)
[full email]

### Email 3: [Subject Line] (Day 4)
[full email]

### Email 4: [Subject Line] (Day 6)
[full email]

### Email 5: [Subject Line] (Day 7)
[full email]

---

## Launch Checklist

### Pre-Launch (Days -7 to -1)
[task list with tools and times]

### Launch Day
[hour-by-hour plan]

### Week 1
[daily action plan]

### Ongoing Cadence
[weekly/monthly plan]

### Month 2-3 Growth
[growth actions]

---

## A/B Test Spec

### Headline Variants
[variants with rationale]

### Price Variants
[variants with rationale]

### Channel Priority
[ranked channels]

### Metrics
[metrics with targets]

### Decision Thresholds
[thresholds with actions]

---

## Kill Criteria

### Week 1
[threshold + action]

### Month 1
[threshold + action]

### Month 3
[threshold + action]

### Qualitative Signals
[signal interpretations]

### Pivot Triggers
[triggers + directions]

---

## References

- **Offer Spec**: [[Admin/Product-Discovery/Offers/{offer-slug}]]
- **Persona**: [[Admin/Product-Discovery/Personas/{persona-slug}]]
- **SWOT Analysis**: [[Admin/Product-Discovery/SWOT/{swot-slug}]]
- **Decision Log**: [[Admin/Product-Discovery/Decisions/{decision-slug}]]
- **Signal Scan**: [[Admin/Product-Discovery/Signal-Scans/{signal-scan-slug}]]
```

---

### Deployment Outputs (Launchpad Repo + Vault)

#### 3. Launchpad Product Branch: `github.com/Peleke/launchpad` branch `{product-slug}`
- Full product directory structure from Phase 3a
- Product README from Phase 3b
- Hero image from Phase 3c (or HTML comment with image intent)
- `emails/` directory with individual email files from Phase 4.6
- PR opened against `main` for operator review (Phase 3d)

#### 4. Content Seeds: `{vault}/Writing/Content-Briefs/{product-slug}-{platform}-seed-{YYYY-MM-DD}.md`
- One file per launch post (Phase 2.4)
- Frontmatter: `type: content-brief`, `status: backlog`, platform tag
- Picked up by content-planner for full brief generation after operator review

#### 5. Launch Checklist Kanban: `{vault}/Admin/Product-Discovery/Pitches/{product-slug}-launch-checklist.kanban.md`
- Obsidian Kanban board with Pre-Launch, Launch Day, Week 1, Ongoing columns (Phase 5.6)
- Tasks have dates, checkable on mobile via iCloud sync

#### 6. Review Reminders: Appended to pitch markdown in vault
- Obsidian Tasks format with `📅` dates and `#hunter/review` tags (Phase 7.6)
- Week 1, Month 1, Month 3 review dates
- Queryable by Obsidian Tasks and Dataview plugins

#### 7. Pipeline Tracker: `{vault}/Admin/Product-Discovery/Pipeline.md`
- Table of all products with branch links, preview URLs, status
- Updated by pitch skill when creating a new product
- Format:

```markdown
| Product | Branch | Preview URL | Price | Status | Pitch Date |
|---------|--------|-------------|-------|--------|------------|
| {name} | [{slug}](https://github.com/Peleke/launchpad/tree/{slug}) | [Preview]({vercel-url}) | ${price} | spec | {date} |
```

---

## Vault Output Contract

All vault output files are written to the Obsidian vault (iCloud-synced):

```
/Users/peleke/Library/Mobile Documents/iCloud~md~obsidian/Documents/ClawTheCurious/
```

**Pitch files:** `Admin/Product-Discovery/Pitches/`
- Filename format: `{product-slug}-pitch-{YYYY-MM-DD}.md` and `{product-slug}-pitch-{YYYY-MM-DD}.json`

**Content seeds:** `Writing/Content-Briefs/`
- Filename format: `{product-slug}-{platform}-seed-{YYYY-MM-DD}.md`

**Launch kanban:** `Admin/Product-Discovery/Pitches/`
- Filename format: `{product-slug}-launch-checklist.kanban.md`

**Pipeline tracker:** `Admin/Product-Discovery/Pipeline.md`

**Rules:**
- If a file already exists at the target path: APPEND under `## Update: {ISO timestamp}`, do NOT overwrite
- Frontmatter MUST include: `type: pitch`, `date`, `status: draft`, `tags` (minimum: `hunter/pitch`, `hunter/domain/{slug}`)

---

## Resources

### references/

- `output-schema.json` -- JSON Schema for the structured pitch output. Load when producing the JSON file to validate against.

### README.md

- The full Launch Architecture framework reference, including intellectual lineage (Ogilvy, Schwartz, Halbert, Godin, Dunford, Fitzpatrick, Lavingia, Hoy, McKenzie), detailed methodology, anti-patterns, and calibration examples. Consult for deep framework questions during a pitch session.

---

## Quality Checklist

Run this checklist before delivering the pitch:

- [ ] Landing page copy is complete and standalone -- someone could build a page from it
- [ ] Every line of landing page copy traces to persona data or offer-scope positioning
- [ ] Headline stops the scroll -- uses persona's exact pain language, not generic marketing
- [ ] Problem section uses minimum 3 persona pain story references with specific language
- [ ] FAQ section maps each Q to a specific persona objection
- [ ] Launch posts follow 80/20 value-first rule -- genuine value even without buying
- [ ] Launch posts follow platform norms (no self-promo spam, platform-specific formatting)
- [ ] Each launch post is ready to publish, not an outline or draft
- [ ] GitHub README is excellent technical documentation first, marketing second (if technical domain)
- [ ] Product structure sketch covers directory layout, file manifest, learning path (if technical domain)
- [ ] Free preview in README is genuinely useful -- not a teaser but real, usable content
- [ ] Email sequence has specific subject lines -- not "Hey!" but specific to the content
- [ ] Each email has ONE job -- no multiple CTAs crammed into a single email
- [ ] Email tone is practitioner-to-practitioner, not marketer-to-mark
- [ ] Launch checklist has specific dates, times, and platforms -- not "post on social media"
- [ ] Launch day plan is hour-by-hour with specific actions
- [ ] A/B test spec has measurable metrics with decision thresholds defined before testing
- [ ] Price variants are grounded in SPEND data, not guesses
- [ ] Kill criteria are time-bounded and measurable -- "sell 10 units in 14 days" not "get traction"
- [ ] Kill criteria distinguish between iterate, pivot, and kill signals
- [ ] All upstream refs linked (offer_ref, persona_ref, swot_ref, decision_ref, signal_scan_ref)
- [ ] JSON output validates against `references/output-schema.json`
- [ ] Markdown output has correct frontmatter (type: pitch, date, status: draft, tags, all refs)
- [ ] Both files are saved to vault `Admin/Product-Discovery/Pitches/`
- [ ] Launchpad branch created with product structure, README, hero image, and emails/
- [ ] PR opened against launchpad main with review checklist
- [ ] Content seeds written to `Writing/Content-Briefs/` with correct frontmatter
- [ ] Launch checklist written as Obsidian kanban with dated tasks
- [ ] Kill criteria review reminders appended with Obsidian Tasks format (📅 dates, #hunter/review)
- [ ] Pipeline tracker updated with new product row
- [ ] All prose content reviewed by buildlog_gauntlet Bragi persona (all rules)
- [ ] Deploy-only mode: all deployment phases either succeeded or were skipped with warnings (no silent failures)
