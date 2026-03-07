---
name: job-search
description: "Job search pipeline — scan for matching roles, sort by interview readiness, generate tailored application briefs, prep submission-ready packages, and track status. Each brief projects the same experience through the lens most relevant to the specific role."
license: MIT
metadata:
  openclaw:
    emoji: "\U0001F3AF"
---

# Job Search Pipeline

Structured job search workflow: scan → sort → brief → package → apply → track.

## When to Use

- Scanning for matching roles across career pages, HN, job boards
- Sorting roles by interview readiness (not just fit)
- Generating tailored application briefs for specific roles
- Creating submission-ready application packages (cover letter, form notes, prep sheet)
- Tracking application status across the pipeline

## Trigger Phrases

- "Scan for matching roles"
- "Sort by interview readiness"
- "Generate a brief for [company]"
- "Prep application package for [company]"
- "Update job search status"
- "/job-search scan"
- "/job-search sort"
- "/job-search brief [company]"
- "/job-search package [company]"

---

## Architecture

### Profiles (5 archetypes)

Each profile is a "view" over the same experience, optimized for a role archetype.
The resume stays the same; the *emphasis* shifts.

| Profile | File | Leads with |
|---------|------|-----------|
| Research Engineer | `profiles/research-engineer.md` | Thompson Sampling, agent learning, evaluation |
| Infrastructure | `profiles/infrastructure.md` | Distributed systems, containment, observability |
| ML / AI Engineer | `profiles/ml-engineer.md` | Retrieval quality, knowledge graphs, benchmarks |
| Developer Tools | `profiles/dev-tools.md` | Framework adapters, MCP, SDK design |
| Product Engineer | `profiles/product-engineer.md` | End-to-end shipping, interactive UI, NLP apps |

### Application Briefs (`applications/`)

Per-company briefs that map candidate strengths to role requirements.

Each brief contains:
- **Lead profile** — which archetype to emphasize
- **Tailored pitch** — 2-3 sentences for this specific role
- **Resume emphasis** — reordered project list for this role
- **"Why this role"** — authentic, specific motivation
- **Proof point mapping** — which evidence matches which requirements
- **Gap analysis** — what's weak and how to frame it
- **"If they ask X, say Y"** — role-specific interview prep

### Application Packages (`applications/*-READY.md`)

Submission-ready packages built from briefs. Each contains:
- **Cover letter** — 300-400 words, specific, no generic filler
- **Application form notes** — pre-filled field values for the platform (Greenhouse/Lever/Ashby)
- **Key links** — portfolio, GitHub, PyPI, articles
- **Interview prep quick sheet** — 5 Q&A pairs, 30-second pitch, question to ask them
- **Pre-submission checklist** — verify before hitting send

### Tracking

- **Targets**: `targets.md` — search results, company list, links
- **Kanban**: Obsidian `ClawTheCurious/Job Search/kanban.md`
- **Statuses**: Researching → Preparing → Applied → Interviewing → Offer → Closed

---

## Workflow: Scan

Search for matching roles across sources.

1. Load profiles to understand candidate strengths
2. Search career pages (Tier 1 weekly, Tier 2 biweekly)
3. Search HN "Who's Hiring" (monthly)
4. Score each role against profiles (1-5 fit)
5. Update `targets.md` with results and links
6. Update Obsidian kanban with new entries

Sources:
- Company career pages (Greenhouse, Ashby, Lever, Workable)
- Hacker News "Who's Hiring" thread
- Y Combinator Work at a Startup
- LinkedIn job alerts

---

## Workflow: Sort

After scanning, sort roles by **interview readiness** — not just fit score.

A 5/5 fit role with a DSA-heavy interview the candidate would fail is LESS valuable than a 4/5 fit role with a portfolio-based interview they'd crush.

### Sort Dimensions

| Dimension | Weight | What It Measures |
|-----------|--------|-----------------|
| Fit score | 2x | How well the role matches candidate strengths |
| Interview style match | 2x | Practical/portfolio vs DSA/competitive vs mixed |
| Speed to offer | 1x | How fast the company typically moves |
| Comp adequacy | 1x | Meets the floor for the location |
| Location fit | 1x | Remote > willing-to-relocate > hard-no |

### Interview Style Categories

| Category | Description | Candidate Readiness |
|----------|-------------|-------------------|
| **Portfolio/practical** | Look at your GitHub, discuss your projects, pair-program on real code | Strongest — the work speaks |
| **System design** | Design a system on a whiteboard in 45 min | Moderate — can do it, needs practice articulating under time pressure |
| **Take-home** | Build something in 4-8 hours, discuss it | Strong — this is what the candidate does daily |
| **ML concepts** | Discuss ML fundamentals, explain your research | Moderate — uses the concepts, needs to be crisper on vocabulary |
| **DSA/leetcode** | Solve algorithmic problems under time pressure | Weakest — needs dedicated ramp time |
| **Math/theory** | Prove convergence, derive equations, discuss proofs | Weakest — practitioner, not theoretician |

### Sort Output

Produce a ranked list with readiness tier:

```markdown
| Rank | Company | Role | Fit | Interview Style | Readiness | Priority |
|------|---------|------|-----|----------------|-----------|----------|
| 1 | {company} | {role} | {n}/5 | Portfolio/practical | Ready now | Apply this week |
| 2 | {company} | {role} | {n}/5 | System design + practical | 2-4 weeks prep | Apply after prep |
| ... | ... | ... | ... | ... | ... | ... |
```

### When to Sort

- After every scan (sort the new results)
- When candidate's readiness changes (DSA improves → re-sort)
- When urgency changes (need income NOW → weight speed-to-offer higher)

---

## Workflow: Brief

Generate a tailored application brief for a specific role.

1. Read the role requirements (from targets.md link or provided JD)
2. Select lead profile based on role requirements
3. Map candidate proof points to role requirements
4. Generate tailored pitch, resume emphasis, and talking points
5. Identify gaps and prepare framing
6. Write brief to `applications/{company-slug}.md`

### Brief Template

```markdown
# {Company} — {Role Title}

## Role Summary
- **Link**: {job posting URL}
- **Location**: {location}
- **Comp**: {compensation range}
- **Fit Score**: {N}/5

## Lead Profile
{which profile to emphasize and why}

## Tailored Pitch (2-3 sentences)
{pitch mapped to this role's specific needs}

## Resume Emphasis
1. {project — why it matters for this role}
2. {project — why it matters for this role}
3. ...

## Proof Point Mapping
| Requirement | Evidence | Strength |
|-------------|----------|----------|
| {requirement from JD} | {specific proof point} | strong/moderate/developing |

## Why This Role (authentic)
{genuine motivation — not boilerplate}

## Gap Analysis
| Gap | How to Frame |
|-----|-------------|
| {missing skill/experience} | {honest framing + growth trajectory} |

## If They Ask X, Say Y
- **"{likely question}"** → {prepared response}
- **"{likely question}"** → {prepared response}

## Application Notes
{any special instructions, referrals, timing}
```

---

## Workflow: Package

Generate a submission-ready application package from an existing brief.

1. Read the brief (`applications/{company-slug}.md`)
2. Fetch the actual job posting for application form details
3. Draft cover letter from brief hooks (300-400 words, specific, no filler)
4. Map application form fields (Greenhouse/Lever/Ashby standard fields)
5. Compile key links (portfolio, GitHub, PyPI, articles)
6. Create interview prep quick sheet (5 Q&A, 30-second pitch, question to ask)
7. Write package to `applications/{company-slug}-READY.md`

### Package Template

```markdown
# {Company} — {Role Title} — Application Package

## Cover Letter
{300-400 words. Every sentence references a specific project or number.
No "I'm passionate about..." — use the role's own language.}

## Application Form Notes
| Field | Value |
|-------|-------|
| Name | Peleke Sengstacke |
| Portfolio | https://peleke.dev |
| GitHub | https://github.com/Peleke |
| {platform-specific fields} | {values} |

## Key Links
| Link | What It Shows |
|------|--------------|
| {url} | {what the reviewer will see} |

## Interview Prep Quick Sheet

### 30-Second Pitch
{tailored to this company}

### Most Likely Questions
1. **"{question}"** → {crisp answer, 2-3 sentences}
...

### Question to Ask Them
{one question that shows you've done homework}

## Pre-Submission Checklist
- [ ] Job posting still live
- [ ] Cover letter references this specific role (not generic)
- [ ] All links work
- [ ] Resume attached
- [ ] Portfolio is clean and loads fast
```

### Cover Letter Rules

- Open with the strongest hook from the brief
- Every sentence must reference a specific project, number, or proof point
- Address the Go/Rust/language gap honestly if applicable (one sentence, not apologetic)
- Close with why THIS company, THIS team — not "I love AI"
- Tone: practitioner writing to practitioners
- 300-400 words. Not longer. Respect their time.

---

## Workflow: Track

Update application status across tracking surfaces.

1. Move kanban card to new column
2. Update targets.md status
3. Add notes (interview dates, feedback, next steps)

---

## Assets

- `assets/proof-points.md` — Master list of citable evidence with numbers
- `assets/anthropic-ramp.md` — 6-9 month interview prep plan for Anthropic
- `profiles/*.md` — Role archetypes with pitches and emphasis orders
- `targets.md` — Company list with search results and links

---

## Quality Checklist

- [ ] Every brief maps to a specific lead profile
- [ ] Pitch uses the role's own language, not generic
- [ ] Proof points are specific numbers, not hand-wavy claims
- [ ] Gap analysis is honest — don't claim expertise you don't have
- [ ] "Why this role" is authentic and role-specific
- [ ] Interview prep covers likely technical deep-dives
- [ ] Sort accounts for interview readiness, not just fit
- [ ] Package cover letter has zero generic sentences
- [ ] Kanban and targets.md stay in sync
