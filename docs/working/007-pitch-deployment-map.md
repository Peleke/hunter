# Pitch Deployment Map: From Markdown to Live Artifacts
**Date**: 2026-02-08
**Status**: Research complete
**Purpose**: Map every pitch output to a deployable artifact, with tooling, integration method, skill ownership, and priority for a solo operator.

---

## The Core Insight

The pitch skill produces 7 phases of go-to-market materials. Today they are markdown files in an Obsidian vault. The goal: each phase should produce (or feed into) something that can be immediately **shown, shared, or sold** -- not documents sitting in a folder.

---

## Deployment Map

```
Pitch Phase            Artifact Type           Deployment Target        Skill Responsible         Integration Method
---------------------------------------------------------------------------------------------------------------------
1. Landing Page Copy   Static HTML site        Vercel preview URL       landing-page-generator    pitch -> HTML template -> git push -> Vercel auto-deploy
2. Launch Posts        Platform-ready text      Obsidian vault (sched)   pitch (extend)            pitch -> vault Content-Briefs/ -> content-planner picks up
3. GitHub README       Git repo + README        GitHub (branch-per-prod) pitch (existing)          pitch -> git branch -> push (already designed)
4. Email Sequence      Email tool import file   ConvertKit / Buttondown  email-deployer (NEW)      pitch -> CSV/JSON import format -> manual or API import
5. Launch Checklist    Actionable task list     Obsidian vault (kanban)  pitch (extend)            pitch -> kanban cards in vault -> visible on mobile
6. A/B Test Spec       Reference document       Obsidian vault           pitch (existing)          stays as reference doc (no deployment needed)
7. Kill Criteria       Reference document       Obsidian vault           pitch (existing)          stays as reference doc (no deployment needed)
```

---

## Detailed Analysis Per Phase

### Phase 1: Landing Page Copy -> Deployed Landing Page

**Current state**: Full landing page copy in markdown (headline, problem, solution, pricing, FAQ, CTA). Ready to paste into a page builder.

**Target state**: An actual deployed HTML page at a shareable URL.

**Deployment options evaluated**:

| Option | Pros | Cons | Setup Time | Ongoing Cost |
|--------|------|------|------------|-------------|
| **Plain HTML + Vercel** | Zero dependencies, full control, fast, free tier | Need a template, no visual editor | 2-3 hrs initial | Free (Vercel hobby) |
| **Astro + Vercel** | Markdown-native, components, fast builds | Framework overhead for a single page | 3-4 hrs initial | Free |
| **Next.js + Vercel** | Ecosystem, easy API routes for future | Overkill for a static landing page | 4-5 hrs initial | Free |
| **Carrd** | No-code, fast, $19/yr | Manual copy-paste, no automation | 30 min per page | $19/yr |
| **Framer/Webflow** | Visual design, nice output | Manual, expensive, no pipeline integration | 1-2 hrs per page | $15-30/mo |

**Recommendation: Plain HTML + Tailwind CSS + Vercel**

Rationale:
- A single `index.html` file with Tailwind CDN is the simplest deployable artifact.
- The `landing-page-generator` skill takes pitch Phase 1 markdown and fills an HTML template. No build step. No framework. Just string interpolation into a well-designed HTML shell.
- Vercel deploys from a git repo. Push a branch, get a preview URL instantly. Each product gets its own branch in a `launchpad` repo (matches the existing launchpad repo pattern from Phase 3).
- A solo operator does not need React to render a sales page.

**Minimum viable integration**:
1. Create a `launchpad` repo with a `/landing-pages/` directory.
2. The `landing-page-generator` skill has an HTML template (single file, Tailwind CDN, responsive).
3. Pitch Phase 1 output gets injected into template sections (headline, problem, solution, pricing, FAQ, CTA).
4. Skill creates a new branch: `product/{product-slug}`.
5. Pushes `index.html` to that branch.
6. Vercel auto-deploys the branch to `product-slug.launchpad.vercel.app` (or similar).
7. Result: a shareable URL within seconds of running the pitch skill.

**Should this be a separate skill?** Yes. `landing-page-generator` is a distinct concern:
- Different input (structured copy sections) from what pitch produces (monolithic markdown).
- Different output (HTML file + git operations) from what pitch does (vault persistence).
- Can be reused independently -- any copy (not just pitch output) can become a landing page.
- Has its own template management (multiple designs, A/B variant pages).

**Dependency graph**:
```
pitch (Phase 1 output)
    |
    v
landing-page-generator
    |
    ├── reads: pitch Phase 1 copy (structured sections)
    ├── reads: HTML template from skill's /templates/ directory
    ├── produces: index.html with injected copy
    ├── pushes: to launchpad repo branch
    └── returns: Vercel preview URL
    |
    v
Vercel (auto-deploy on push)
    |
    v
Live URL (shareable immediately)
```

**Template structure for `landing-page-generator`**:
```
landing-page-generator/
  SKILL.md
  templates/
    default.html          # Clean single-page layout, Tailwind CDN
    minimal.html          # Ultra-simple, no images, text-focused
    technical.html        # Code-block friendly, dark mode option
  references/
    section-mapping.json  # Maps pitch output sections -> template slots
```

**Future enhancements** (not MVP):
- Multiple templates selectable per product.
- Automatic Gumroad/LemonSqueezy embed insertion (buy button).
- ConvertKit form embed for email capture.
- OG meta tags auto-generated from headline + subheadline.
- Analytics snippet (Plausible or Fathom -- privacy-respecting, solo-operator-friendly).

---

### Phase 2: Launch Posts -> Scheduled/Published Content

**Current state**: Platform-specific posts (Reddit, LinkedIn, Twitter/X) as markdown text. Ready to paste manually.

**Target state**: Posts written to the Obsidian vault in a format that the `content-planner` skill can pick up, schedule, and track.

**Deployment options evaluated**:

| Option | Pros | Cons | Setup Time | Ongoing Cost |
|--------|------|------|------------|-------------|
| **Vault + content-planner** | Already designed, vault-as-database, mobile visible | Manual publish (copy-paste to platform) | 0 hrs (exists) | Free |
| **Buffer/Hootsuite** | Auto-publish, scheduling | API setup, another SaaS, cost | 1-2 hrs | $6-15/mo |
| **LinkedIn API direct** | True automation | Complex OAuth, rate limits, fragile | 4-6 hrs | Free (API) |
| **Linwheel** | LinkedIn scheduling | Another tool, limited to LI | 1 hr | Varies |
| **Typefully** | Twitter thread scheduling | Twitter-only, another tool | 30 min | $12/mo |

**Recommendation: Vault + content-planner (extend pitch to write Content-Briefs)**

Rationale:
- The `content-planner` skill already reads `Writing/Content-Briefs/` and outputs kanban + schedule.
- Pitch Phase 2 posts are content briefs. They just need the right frontmatter.
- A solo operator doing 2-3 launches is not going to set up LinkedIn OAuth. Copy-paste from the vault is 30 seconds per post.
- The vault is visible on mobile via iCloud sync. Open Obsidian on phone, copy, paste into LinkedIn app. Done.

**Minimum viable integration**:
1. Pitch Phase 2, instead of (or in addition to) writing posts to the pitch markdown file, writes each post as a separate Content Brief to `Writing/Content-Briefs/`.
2. Frontmatter includes:
   ```yaml
   type: content-brief
   status: backlog
   series: "launch/{product-slug}"
   channel: reddit  # or linkedin, twitter
   source_pitch: "{product-slug}-pitch-{date}"
   launch_date: YYYY-MM-DD  # target publish date
   ready: true
   tags:
     - content/brief
     - content/channel/reddit
     - hunter/pitch
   ```
3. The content-planner picks these up in its next scan and slots them into the kanban/schedule.
4. Operator publishes manually from the vault (copy-paste).

**Should this be a separate skill?** No. This is an extension of `pitch`. The pitch skill already produces the posts -- it just needs to also write them as Content Briefs. Adding a vault-write step to Phase 2 is ~20 lines of logic, not a new skill.

**Future enhancements** (not MVP):
- A `publish` skill that takes a Content Brief and posts it via API (LinkedIn, Reddit, Twitter). This is worth building only after the operator has a repeatable cadence (10+ posts published manually). Building API integrations before validating the content cadence is premature optimization.
- Integration with scheduling tools (Buffer, Typefully) via their APIs -- but only if manual copy-paste becomes a genuine bottleneck. For a solo operator doing 3-5 posts per week, it will not be.

---

### Phase 3: GitHub README -> Already Deployed

**Current state**: Full README.md content + product structure sketch (directory layout, file manifest). Designed for a companion GitHub repo.

**Target state**: Already designed. The launchpad repo pattern (each branch = a product) is described in the pitch output. Phase 3 is the one phase that already maps to a real deployable artifact.

**Minimum viable integration** (already described in pitch):
1. Create the GitHub repo (e.g., `devops-decision-kit`).
2. Push the README from Phase 3b.
3. Create the directory structure from Phase 3a.
4. Populate free-tier content files.
5. The repo IS the artifact. No additional tooling needed.

**Connection to landing-page-generator**: The landing page links to the GitHub repo. The GitHub repo links to the landing page. They reinforce each other.

**Should this be a separate skill?** No. The pitch already produces the complete README content. The manual step is `git init && git push`. Automating this is not worth a skill -- it is a 5-minute terminal operation done once per product.

---

### Phase 4: Email Sequence -> Email Tool Import

**Current state**: 5 emails with subject lines, body copy, and send timing (Day 0, 2, 4, 6, 7). Ready to paste into an email tool.

**Target state**: A file format that can be imported directly into ConvertKit or Buttondown, minimizing manual copy-paste.

**Deployment options evaluated**:

| Option | Pros | Cons | Setup Time | Ongoing Cost |
|--------|------|------|------------|-------------|
| **Manual paste into ConvertKit** | No setup, works now | Tedious for 5 emails, error-prone | 0 hrs setup | Free (CK free tier) |
| **ConvertKit API import** | True automation | API setup, OAuth, fragile if CK changes | 3-4 hrs | Free (CK free tier) |
| **Buttondown API** | Simpler API than CK, markdown-native | Smaller ecosystem, fewer automation features | 2-3 hrs | Free tier or $9/mo |
| **Generate CK-importable format** | No API needed, just structured output | Still some manual steps | 1 hr | Free |

**Recommendation: Generate structured email files + manual import (for now)**

Rationale:
- ConvertKit's free tier handles up to 10,000 subscribers with basic automation sequences.
- Buttondown is markdown-native, which fits the vault-as-database pattern better.
- The real bottleneck is not "importing 5 emails" -- it is setting up the automation triggers (purchase -> tag -> sequence start). That is manual regardless.
- For a solo operator launching 1-2 products, pasting 5 emails into ConvertKit takes 15 minutes. Building API automation for 15 minutes of work is not worth it.

**Minimum viable integration**:
1. Pitch Phase 4 outputs each email as a separate markdown file in the vault:
   ```
   Admin/Product-Discovery/Pitches/{product-slug}/
     email-1-welcome.md
     email-2-quick-win.md
     email-3-story.md
     email-4-objection.md
     email-5-community.md
   ```
2. Each file has frontmatter:
   ```yaml
   type: email
   sequence: "{product-slug}-post-purchase"
   position: 1
   send_delay_days: 0
   subject: "Your DevOps Decision Kit is ready -- open the Quick Start first"
   tags: [hunter/pitch, hunter/email]
   ```
3. Operator opens each file, copies body into ConvertKit/Buttondown sequence editor.

**Should this be a separate skill?** Not yet. When the operator has 3+ products each with 5-email sequences, and is switching email tools, then an `email-deployer` skill that reads structured email files and pushes them via API becomes worth building. Until then: manual paste.

**Trigger for building `email-deployer`**: When the operator spends more than 30 minutes per launch on email import, it is time to automate.

**Future enhancements**:
- `email-deployer` skill that:
  - Reads email files from the vault.
  - Pushes to ConvertKit via API (create sequence, add emails, set delays).
  - Returns confirmation with sequence URL.
- Buttondown integration (simpler API, markdown-native -- may be the better long-term choice for a markdown-first pipeline).
- A/B subject line variants included in the email files, pushed as CK A/B tests.

---

### Phase 5: Launch Checklist -> Vault Kanban + Task List

**Current state**: Hour-by-hour execution plan as markdown. Pre-launch tasks, launch day timeline, week 1 follow-up, ongoing cadence.

**Target state**: Actionable tasks visible in the Obsidian vault as a kanban board or task list, checkable on mobile.

**Recommendation: Write as Obsidian tasks in a dedicated launch kanban**

Rationale:
- Obsidian has kanban plugin (already in use for Pipeline.kanban.md and Content-Plan.kanban.md).
- Tasks plugin can aggregate tasks across files.
- The operator already lives in Obsidian. Adding another tool (Notion, Todoist, Linear) fragments attention.

**Minimum viable integration**:
1. Pitch Phase 5 writes a launch-specific kanban:
   ```
   Admin/Product-Discovery/Pitches/{product-slug}/Launch-Plan.kanban.md
   ```
2. Columns: `Pre-Launch | Launch Day | Week 1 | Ongoing | Done`
3. Each task has a due date, time, and platform:
   ```markdown
   - [ ] Set up Gumroad product page @{2026-02-15} #tool/gumroad
   - [ ] Publish r/devops post @{2026-02-22 09:00} #platform/reddit
   ```
4. Visible on mobile via iCloud sync. Check off tasks as completed.

**Should this be a separate skill?** No. This is a formatting extension of pitch Phase 5. The checklist content already exists; it just needs to be output as kanban-compatible markdown instead of (or in addition to) a flat list.

---

### Phase 6: A/B Test Spec -> Reference Document (No Deployment)

**Current state**: Headline variants, price variants, channel priority, metrics, decision thresholds. All as markdown.

**Target state**: Stays as a reference document. This is a plan, not a deployable artifact.

**Why no deployment integration**: A/B testing at solo-operator scale (100-500 visitors) is sequential, not automated. You change the headline manually. You check Gumroad analytics manually. There is no tool to deploy this to -- you are the tool.

**One useful connection**: When `landing-page-generator` exists, it could generate variant landing pages (Headline A page, Headline B page) as separate Vercel preview URLs. The operator shares different URLs in different channels. This is a future enhancement, not MVP.

---

### Phase 7: Kill Criteria -> Reference Document (No Deployment)

**Current state**: Time-bounded thresholds with actions. Week 1, Month 1, Month 3 checkpoints.

**Target state**: Stays as a reference document, but with one addition: scheduled review reminders in the vault.

**Minimum viable integration**:
1. When pitch writes kill criteria to the vault, it also creates reminder tasks:
   ```markdown
   - [ ] Week 1 kill criteria review @{launch_date + 7 days} #hunter/review
   - [ ] Month 1 kill criteria review @{launch_date + 30 days} #hunter/review
   - [ ] Month 3 kill criteria review @{launch_date + 90 days} #hunter/review
   ```
2. These appear in Obsidian's task aggregation. The operator gets reminded to check the numbers.

---

## Content Sketches -> Vault for content-planner

This was mentioned in the brief but is not a numbered pitch phase. It refers to content ideas that emerge during the pitch process (blog post angles, LinkedIn series ideas, etc.).

**Integration**: Already handled. The `content-planner` skill reads `Writing/Content-Briefs/`. If the pitch or any other skill writes a content brief to that directory with proper frontmatter, content-planner picks it up. No new skill needed.

---

## Slide Decks (Future Possibility -- Noted, Not Prioritized)

The user flagged this as "probably overkill." Agreed. A slide deck generator would:
- Take pitch Phase 1 copy and convert it to a presentation format.
- Use a tool like Slidev (markdown-to-slides) or Marp.
- Deploy as a web presentation at a shareable URL.

**Why not now**: A solo operator selling a $29 info product does not need a pitch deck. Slide decks become relevant if/when:
- The operator presents at meetups or conferences (then Slidev/Marp is worth building).
- The community (Skool) needs presentation materials for live sessions.
- A B2B pivot happens and enterprise buyers want slide decks.

**Noted for**: The `landing-page-generator` template system could be extended to produce Slidev markdown. Same skill, different template. But this is 6+ months out.

---

## Dependency Graph (Complete)

```
                        signal-scan
                            |
                        decision-log
                            |
                       persona-extract
                          /    \
                   swot-analysis  content-planner  <--- reads vault Content-Briefs/
                         |              |
                     offer-scope        |
                         |              |
                       pitch -----------+--- writes Content-Briefs/ (Phase 2 posts)
                      /  |  \
                     /   |   \
                    /    |    \
    landing-page-     (vault    (vault
    generator         emails)    kanban)
        |
    launchpad repo
    (git branch)
        |
    Vercel auto-deploy
        |
    Live preview URL
```

---

## Priority Matrix: What to Build and When

### Tier 1: Build Now (High Impact, Low Effort)

| Integration | Impact | Effort | Why Now |
|-------------|--------|--------|---------|
| **Pitch -> Content Briefs (Phase 2 extension)** | High: launch posts become scheduled, tracked, visible on mobile | Low: add vault-write step to existing pitch output | The content-planner already exists. This is wiring, not building. |
| **Pitch -> Launch Kanban (Phase 5 extension)** | Medium: actionable tasks instead of a flat list | Low: reformat existing output as kanban markdown | Obsidian kanban plugin is already in use. Same pattern. |
| **Pitch -> Kill Criteria Reminders (Phase 7 extension)** | Medium: prevents forgetting to review thresholds | Trivial: 3 task lines with dates | 5 minutes of work. Enormous value if it prevents sunk-cost fallacy. |

### Tier 2: Build Next (High Impact, Medium Effort)

| Integration | Impact | Effort | Why Next |
|-------------|--------|--------|----------|
| **landing-page-generator skill** | Very High: every pitch produces a live URL | Medium: HTML template + git push logic + Vercel setup | This is the single highest-leverage new skill. A shareable URL is worth 10x a markdown file. But it requires designing the template, setting up the launchpad repo, and connecting Vercel. Estimate: 4-6 hours for MVP. |
| **Pitch -> Email files in vault (Phase 4 extension)** | Medium: structured email files instead of monolithic markdown | Low-Medium: split email output into individual files with frontmatter | Useful for organization. Not urgent until the operator is juggling multiple product email sequences. |

### Tier 3: Build Later (Medium Impact, Higher Effort)

| Integration | Impact | Effort | Why Later |
|-------------|--------|--------|-----------|
| **email-deployer skill** | Medium: automates ConvertKit/Buttondown import | High: API integration, auth, error handling | Not worth building until there are 3+ products with email sequences. Manual paste is fine for 1-2 launches. |
| **publish skill** | Medium: auto-posts to LinkedIn/Reddit/Twitter | High: multiple API integrations, OAuth, rate limits | Not worth building until the operator has a proven content cadence. API auth for 3 platforms is a maintenance burden. Build only when manual copy-paste becomes a genuine bottleneck (10+ posts/week). |
| **A/B variant page generation** | Low-Medium: auto-generates variant landing pages | Medium: extends landing-page-generator | Only useful after there is enough traffic to test variants. Premature before 200+ landing page visitors. |

### Tier 4: Someday/Maybe (Low Urgency)

| Integration | Impact | Effort | Notes |
|-------------|--------|--------|-------|
| **Slide deck generator** | Low | Medium | Marp or Slidev. Only if presenting at events or running community sessions. |
| **Gumroad/LemonSqueezy API integration** | Low | Medium | Auto-create product pages. Manual setup takes 15 minutes. Not worth automating. |
| **Analytics dashboard** | Low | High | Aggregate Gumroad + GitHub + email metrics. Use each tool's native dashboard for now. |

---

## Skill Inventory: New vs Extended

### New Skills to Build

| Skill | Description | Priority | Dependencies |
|-------|-------------|----------|-------------|
| `landing-page-generator` | Takes pitch Phase 1 copy, injects into HTML template, pushes to git, returns Vercel URL | Tier 2 (build next) | `pitch` output, `launchpad` repo, Vercel account |

### Existing Skills to Extend

| Skill | Extension | Priority |
|-------|-----------|----------|
| `pitch` | Phase 2: also write posts as Content Briefs to vault `Writing/Content-Briefs/` | Tier 1 |
| `pitch` | Phase 5: also write launch plan as kanban to vault | Tier 1 |
| `pitch` | Phase 7: also write review reminder tasks with dates | Tier 1 |
| `pitch` | Phase 4: also write individual email files with frontmatter | Tier 2 |

### Skills NOT to Build (Yet)

| Skill | Why Not |
|-------|---------|
| `email-deployer` | Manual paste is fine for 1-2 products. Build at 3+ products. |
| `publish` | API integrations for 3 platforms are a maintenance burden. Build when manual copy-paste is a bottleneck at 10+ posts/week. |
| `slide-generator` | No use case yet. Note for future. |

---

## Platform/Tool Summary

| Tool | Purpose | Cost | When Needed |
|------|---------|------|-------------|
| **Vercel** | Landing page hosting, preview URLs | Free (hobby tier) | When `landing-page-generator` is built |
| **GitHub** | Product repo (README + templates), launchpad repo | Free | Now (already in use) |
| **ConvertKit** | Email sequences, automation | Free (up to 10K subscribers) | At launch |
| **Buttondown** | Alternative email tool (markdown-native) | Free tier or $9/mo | If CK does not fit |
| **Gumroad** | Payment processing, product delivery | Free (10% fee per sale) | At launch |
| **LemonSqueezy** | Alternative payment (better EU VAT handling) | Free (5% + $0.50 per sale) | Alternative to Gumroad |
| **Obsidian** | Vault-as-database, kanban, mobile access | Free (iCloud sync) | Now (already in use) |
| **Tailwind CSS** | Landing page styling (CDN, no build step) | Free | When `landing-page-generator` is built |
| **Plausible/Fathom** | Privacy-respecting analytics for landing pages | $9/mo (Plausible) | After first landing page is live |

---

## The Vault-as-Hub Pattern

Everything routes through the Obsidian vault. This is by design:

```
pitch output
    |
    +---> vault: Pitches/              (the pitch itself)
    +---> vault: Content-Briefs/       (launch posts for content-planner)
    +---> vault: Pitches/{slug}/       (email files, launch kanban)
    +---> launchpad repo               (landing page HTML, README)
    |         |
    |         +---> Vercel             (live URL)
    |         +---> GitHub             (public repo)
    |
    +---> ConvertKit/Buttondown        (email sequences -- manual for now)
    +---> Gumroad/LemonSqueezy         (product page -- manual for now)
```

The vault is the single source of truth. External tools (Vercel, ConvertKit, Gumroad) are deployment targets that receive data FROM the vault pipeline. They do not feed back into it (yet). When they do (analytics, sales data), that data flows back into the vault as frontmatter fields on the pitch document, enabling dataview queries like "show all products with Month 1 revenue > $200."

---

## Implementation Sequence

**Week 1**: Extend `pitch` (Tier 1 items)
1. Add Content Brief output to Phase 2 (launch posts -> `Writing/Content-Briefs/`).
2. Add kanban output to Phase 5 (launch checklist -> `Pitches/{slug}/Launch-Plan.kanban.md`).
3. Add reminder tasks to Phase 7 (kill criteria -> dated review tasks).

**Week 2**: Build `landing-page-generator` (Tier 2)
1. Create HTML template (single-page, Tailwind CDN, responsive).
2. Build the skill: read pitch Phase 1 output, inject into template, write `index.html`.
3. Set up `launchpad` repo on GitHub.
4. Connect Vercel to the repo.
5. Test: run pitch + landing-page-generator on DevOps Decision Kit -> get a live URL.

**Week 3+**: Extend as needed based on actual launch experience.

---

## Decision Log

| Decision | Rationale | Revisit When |
|----------|-----------|-------------|
| Plain HTML over Astro/Next.js for landing pages | Solo operator, single-page sites, no build step needed. Tailwind CDN + template is simpler. | When landing pages need dynamic features (forms, auth, dashboards). |
| Vault + manual publish over API integrations for social posts | API auth for 3 platforms is maintenance debt. Manual copy-paste from mobile is 30 seconds. | When posting cadence exceeds 10 posts/week. |
| ConvertKit over Buttondown for email | Larger ecosystem, better automation triggers, free tier to 10K subs. | If markdown-native email becomes important (Buttondown is better for that). |
| No slide deck generator | No current use case. Landing page + posts + email covers the launch. | When presenting at events or running community sessions. |
| `landing-page-generator` as separate skill, not pitch extension | Different concern (HTML generation + git + deploy), different inputs (structured sections vs. monolithic copy), reusable for non-pitch copy. | If the skill is only ever called by pitch and never standalone. |
