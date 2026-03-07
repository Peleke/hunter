---
name: landing-page
description: "Generate a single-file HTML landing page from pitch output. Use when a validated pitch needs a deployable dark-theme page on the launchpad repo."
license: MIT
metadata:
  openclaw:
    emoji: "\U0001F310"
---

# Landing Page

Generate a complete, single-file HTML landing page from pitch output. Reads Phase 1 copy from a pitch document in the vault, renders it as semantic HTML with an inline dark-theme design system, and commits the result to a product-specific branch on the launchpad repo. Zero external dependencies, mobile responsive, under 30KB.

This skill formalizes what was previously Phase 3e of the pitch skill into a standalone, reusable skill. It can be triggered independently after any pitch is finalized.

## Step 0: Load Conventions

Before doing ANYTHING, read the shared conventions file:

```
Read ${SKILLS_DIR}/_conventions.md
```

This file defines: canonical vault path, folder-to-type mapping, frontmatter contract, valid statuses, tag hierarchy, cross-reference syntax, and the PipelineEnvelope schema. All output from this skill MUST conform to those conventions. If there is a conflict between this SKILL.md and `_conventions.md`, the conventions file wins.

## Pipeline Position

```
signal-scan -> decision-log -> persona-extract -> swot-analysis -> offer-scope -> pitch -> **landing-page** -> one-pager -> hunter-log
```

This skill consumes the output of `pitch` and feeds into `one-pager`. The landing page is the first deployable artifact from the pitch -- a live page that validates copy with real visitors before the operator invests in PDF one-pagers or slide decks.

## When to Use

- After pitch completes: generate a deployable landing page from the pitch output
- Re-deploying a landing page after editing pitch copy
- Generating a standalone landing page for any product with structured pitch data
- When the operator says "make it live" after reviewing pitch copy

## Trigger Phrases

- "Generate a landing page from this pitch"
- "Build the landing page for [product]"
- "Deploy a landing page for [product-slug]"
- "Make the pitch live"
- "/landing-page [pitch-slug]"

---

## Prerequisites

Before starting, the following must be available:

1. **Pitch output** -- A completed pitch result in the vault (`Admin/Product-Discovery/Pitches/{slug}.md` or `.json`) containing the `landing_page` section with: headline, subheadline, problem_section, solution_section, whats_inside, social_proof_strategy, pricing_section, faq, primary_cta, secondary_cta
2. **Launchpad repo** -- The `launchpad` repo must be cloned locally at `${HUNTER_DIR}/../launchpad` or `~/Documents/Projects/launchpad/`. If the product branch does not exist yet, this skill will create it.

If the pitch output is missing or incomplete, prompt the user to run the pitch skill first.

---

## Input

The skill expects pitch output (JSON or markdown) with a `landing_page` section:

```typescript
interface LandingPageInput {
  pitch_ref: string               // slug of the pitch document
  product_name: string
  product_slug: string            // kebab-case, used for branch name and URLs
  landing_page: {
    headline: string
    subheadline: string
    problem_section: {
      prose: string[]             // pain-point paragraphs
      quotes: {
        text: string
        attribution: string
      }[]
    }
    solution_section: {
      before: { heading: string, items: string[] }
      after: { heading: string, items: string[] }
    }
    how_it_works?: {              // optional ordered steps
      step: number
      title: string
      description: string
    }[]
    whats_inside: {
      columns: string[]           // table column headers
      rows: { module: string, description: string, time?: string }[]
    }
    social_proof_strategy: string
    pricing_section: {
      tiers: {
        name: string
        price: string
        features: string[]
        featured?: boolean
      }[]
      guarantee?: string
    }
    faq: { question: string, answer: string }[]
    primary_cta: { text: string, url?: string }
    secondary_cta?: { text: string, url?: string }
  }
  hero_image_exists: boolean      // whether hero.png is on the branch
  domain: string
  opportunity: string
}
```

---

## Workflow

```
Pitch output in vault (JSON or markdown)
    |
Phase 1: Locate and parse pitch data
    |
Phase 2: Generate index.html with inline design system
    |
Phase 3: Deploy to launchpad branch
    |
Phase 4: Write vault reference note
    |
Output: index.html on launchpad branch + vault note
```

---

## Phase 1: Locate and Parse Pitch Data

Find the pitch output in the vault:

```
Glob: ${VAULT}/Admin/Product-Discovery/Pitches/{product-slug}*.md
Glob: ${VAULT}/Admin/Product-Discovery/Pitches/{product-slug}*.json
```

If both exist, prefer the JSON (structured data is easier to parse). If only the markdown exists, parse it by H2/H3 section headers following the pitch parsing contract (match on section name, ignoring any `Phase N:` prefix).

Extract the `landing_page` section and validate that all required fields are present: headline, subheadline, problem_section, solution_section, whats_inside, pricing_section, faq, primary_cta. Warn (do not fail) if optional fields are missing.

---

## Phase 2: Generate index.html

Generate a single `index.html` file. All CSS is inline in one `<style>` tag. No JavaScript, no CDN, no external fonts. The file must work as a local `file://` page AND when served by Vercel.

### 2.1 Design System

Generate a single `<style>` tag inside `<head>` with these design tokens and components:

**Color tokens (CSS custom properties on `:root`):**

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#0d1117` | Page background |
| `--surface` | `#161b22` | Cards, blockquotes, FAQ panels |
| `--border` | `#30363d` | All borders |
| `--text` | `#e6edf3` | Primary text |
| `--text-muted` | `#8b949e` | Secondary text, descriptions, citations |
| `--accent` | `#58a6ff` | Links, module names, interactive highlights |
| `--accent-hover` | `#79c0ff` | Link hover state |
| `--green` | `#3fb950` | Primary CTA buttons, "after" cards, checkmarks |
| `--orange` | `#d29922` | Warning or emphasis accents |
| `--red` | `#f85149` | "Before" cards, problem emphasis |

The accent color is configurable per-product. If the pitch specifies a brand color, override `--accent` and `--accent-hover`. All other tokens remain fixed across products.

**Typography:**
- Font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif`
- Line height: `1.6`
- `-webkit-font-smoothing: antialiased`
- Hero h1: `2.8rem`, weight `800`, letter-spacing `-0.02em`
- Section h2: `1.8rem`, weight `700`, letter-spacing `-0.01em`
- Body text: `1.05rem`, color `var(--text-muted)`

**Layout:**
- `.container`: max-width `720px`, centered, `24px` horizontal padding
- Sections separated by `border-top: 1px solid var(--border)` with `60px` vertical padding
- Mobile breakpoint at `640px`

### 2.2 Section-to-HTML Mapping

Each pitch section maps to a semantic HTML block. Generate every section present in the pitch. If a section is empty or was not generated, skip it -- do NOT generate placeholder content.

| Pitch Section | HTML Element | Key Styling |
|---------------|-------------|-------------|
| **Headline + Subheadline** | `<header class="hero">` containing `<h1>` and `<p class="sub">` | Centered, `80px` top padding, `60px` bottom |
| **Primary + Secondary CTA** | `<div class="cta-group">` with `<a class="btn btn-primary">` and `<a class="btn btn-secondary">` | Flex row, centered, wraps on mobile. Primary: green bg, black text. Secondary: surface bg with border. |
| **Problem Section** | `<section>` with prose `<p>` tags and `<blockquote>` elements | Blockquotes: `3px` left border in accent color, surface bg, `6px` right border-radius. Attribution in `<cite>`. |
| **Solution / Before-After** | `<section>` with `.before-after` grid (`1fr 1fr`) containing `.ba-card.before` and `.ba-card.after` | Before: red-tinted border/heading. After: green-tinted. Collapse to single column on mobile. |
| **How It Works** | `<ol class="steps">` with `<li>` items | CSS counter with numbered circles (accent text, surface bg) positioned left of each step. |
| **What's Inside / Modules** | `<table class="modules-table">` with `<thead>` and `<tbody>` | First column: accent color, `600` weight, no-wrap. Last column: muted, smaller. Hover: surface bg. |
| **Social Proof / Credibility** | `<section>` with `<div class="cred">` | Surface bg, bordered card, `28px` padding. |
| **Pricing** | `<section>` with `.pricing-cards` grid (`1fr 1fr`) | Each card: surface bg, bordered. Featured: green border. Price: `2rem` weight `800`. Checkmark list via `li::before`. |
| **FAQ** | `<details>` elements with `<summary>` and `<div>` | Surface bg, bordered, `6px` radius. Custom marker: `+`/`-`. No default webkit marker. |
| **Final CTA** | `<div class="final-cta">` with h2 + `<p>` + `.cta-group` | Centered, `60px` padding, mirrors hero CTA. |
| **Footer** | `<footer>` | Centered, muted, `40px` padding. Links to GitHub and author. |

### 2.3 Open Graph and Meta Tags

Generate in `<head>`:

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{Product Name} -- {Headline (truncated to ~60 chars)}</title>
<meta name="description" content="{Subheadline, max 160 chars}">
<meta property="og:title" content="{Product Name} -- {Headline}">
<meta property="og:description" content="{Subheadline}">
<meta property="og:type" content="website">
<meta property="og:url" content="https://launchpad-git-{product-slug}-kwayet-fs-projects.vercel.app">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{Product Name} -- {Headline}">
<meta name="twitter:description" content="{Subheadline}">
```

If a hero image exists on the branch (`hero.png`), add:
```html
<meta property="og:image" content="/hero.png">
<meta name="twitter:image" content="/hero.png">
```

If not, omit rather than linking to a nonexistent file.

### 2.4 CTA Link Targets

| CTA | Link Target | Fallback |
|-----|------------|----------|
| Primary | `https://github.com/Peleke/launchpad/tree/{product-slug}` | `#` with `<!-- TODO: update link -->` |
| Secondary | Skool community URL if known | `#community` anchor |
| Email signup | ConvertKit/Buttondown URL if known | `#` with `<!-- TODO: add signup link -->` |

Every placeholder link must have an HTML comment explaining what URL to replace it with.

### 2.5 Content Fidelity Rules

- **Do NOT rewrite copy.** The HTML renders pitch Phase 1 copy verbatim.
- **Typographic entities required.** Straight quotes become curly (`&ldquo;`/`&rdquo;`, `&lsquo;`/`&rsquo;`), hyphens become em-dashes (`&mdash;`), three dots become ellipsis (`&hellip;`).
- **Preserve all persona quotes.** Every blockquote must include quote text in `<p>` and attribution in `<cite>` with em-dash prefix.
- **Preserve the module/feature table exactly.** Same columns, same rows as the pitch.
- **Preserve all FAQ items.** Each FAQ becomes one `<details>/<summary>` element. Do not merge or split.

### 2.6 Responsive Behavior

The `@media (max-width: 640px)` breakpoint must:
- Reduce hero h1 to `2rem`, subtitle to `1.05rem`
- Collapse `.before-after` and `.pricing-cards` grids to single column
- Tighten module table padding (`10px 8px`) and font (`0.85rem`)
- Reduce container padding to `16px`
- No horizontal scrolling at any viewport width down to `320px`

---

## Phase 3: Deploy to Launchpad Branch

1. **Check for launchpad repo** at `~/Documents/Projects/launchpad/`
2. **Check if product branch exists**: `git branch -a | grep {product-slug}`
   - If yes: checkout the branch
   - If no: create a new branch from main: `git checkout -b {product-slug}`
3. **Write `index.html`** to the branch root
4. **Commit**: `git add index.html && git commit -m "feat: add landing page for {Product Name}"`
5. **Push** (if remote is configured): `git push -u origin {product-slug}`
6. **Open PR** (if no PR exists for this branch): `gh pr create --title "{Product Name}: Landing Page" --body "..."`

The PR body should include:
- Preview URL: `https://launchpad-git-{product-slug}-kwayet-fs-projects.vercel.app`
- Validation checklist (see Quality Checklist below)
- Link back to pitch document in vault

If a PR already exists for this branch, this becomes an additional commit on the same PR. Do NOT open a duplicate PR.

---

## Phase 4: Write Vault Reference Note

Write an Obsidian note to track the generated landing page:

**Path**: `${VAULT}/Admin/Product-Discovery/Landing-Pages/{product-slug}-landing-page-{YYYY-MM-DD}.md`

**Frontmatter**:
```yaml
---
type: landing-page
date: YYYY-MM-DD
status: draft
product: PRODUCT NAME
pitch_ref: "{pitch-slug}"
branch: "{product-slug}"
preview_url: "https://launchpad-git-{product-slug}-kwayet-fs-projects.vercel.app"
tags:
  - hunter/landing-page
  - hunter/domain/{domain-slug}
---
```

**Body**:
```markdown
## Landing Page

Generated from [[Admin/Product-Discovery/Pitches/{pitch-slug}]].

- Branch: [{product-slug}](https://github.com/Peleke/launchpad/tree/{product-slug})
- Preview: [Live Page]({preview-url})
- File: `index.html` ({file-size} KB)
- Sections: {list of rendered section types}

## Validation
- [ ] Renders at 320px without horizontal scroll
- [ ] All pitch sections present
- [ ] OG tags correct
- [ ] CTA links updated from placeholders
```

**Rules:**
- If a file already exists at the target path: APPEND under `## Update: {ISO timestamp}`, do NOT overwrite
- Frontmatter MUST include: `type: landing-page`, `date`, `status: draft`, `tags` (minimum: `hunter/landing-page`, `hunter/domain/{slug}`)

---

## Output

### Primary Output

#### 1. Landing Page HTML: `index.html` on launchpad product branch

- Single-file HTML landing page with inline CSS
- Dark theme, mobile responsive, zero dependencies
- Under 30KB
- Open Graph meta tags for social sharing
- Immediately deployable via Vercel branch preview
- Preview URL: `https://launchpad-git-{product-slug}-kwayet-fs-projects.vercel.app`

### Vault Output

#### 2. Reference Note: `${VAULT}/Admin/Product-Discovery/Landing-Pages/{product-slug}-landing-page-{YYYY-MM-DD}.md`

- Obsidian note linking to the live page, branch, and upstream pitch
- Validation checklist for operator review
- Frontmatter following the conventions contract

### PipelineEnvelope

```json
{
  "skill": "landing-page",
  "version": "1.0",
  "session_id": "session-YYYY-MM-DD-NNN",
  "timestamp": "ISO 8601",
  "input_refs": ["pitch-slug"],
  "output": {
    "landing_page_id": "{product-slug}-landing-page-{YYYY-MM-DD}",
    "product_name": "...",
    "html_file_path": "index.html",
    "branch_name": "{product-slug}",
    "file_size_bytes": 0,
    "has_hero_image": false,
    "og_tags": { "title": "...", "description": "..." },
    "sections_rendered": ["hero", "problem", "solution", "modules", "pricing", "faq", "footer"],
    "responsive_validated": true,
    "vault_note_path": "Admin/Product-Discovery/Landing-Pages/{product-slug}-landing-page-{YYYY-MM-DD}.md"
  }
}
```

---

## Pipeline Kanban Contract

Landing page output does NOT move the kanban card. The card stays in whatever column pitch left it in (typically "Offer Scoped"). The operator moves it to "Building" manually when they set a launch date.

---

## Quality Checklist

Run this checklist before delivering the landing page:

- [ ] Valid HTML5 (`<!DOCTYPE html>`, `<html lang="en">`)
- [ ] All CSS inline in a single `<style>` tag -- no external stylesheets
- [ ] No JavaScript -- pure HTML + CSS
- [ ] No external dependencies -- no CDN, no Google Fonts, no analytics
- [ ] All pitch sections present (unless empty in the pitch)
- [ ] All persona quotes as `<blockquote>` with `<cite>`
- [ ] Module/feature table matches pitch exactly
- [ ] All FAQs as `<details>/<summary>`
- [ ] Both CTAs in hero and final CTA sections
- [ ] Open Graph meta tags with correct product name and description
- [ ] Mobile responsive at 640px (no horizontal scroll, grids collapse)
- [ ] No horizontal scroll at any width down to 320px
- [ ] All placeholder links have HTML comments
- [ ] Typographic entities used throughout (curly quotes, em-dashes, ellipsis)
- [ ] File under 30KB
- [ ] Committed to correct launchpad branch
- [ ] Vault reference note written with correct frontmatter
- [ ] All prose reviewed by buildlog_gauntlet Bragi persona (if copy was modified)

---

## Auto-Persist

After all phases complete: wrap output in PipelineEnvelope, invoke `hunter-log` to persist the vault note and update the kanban board. Do NOT ask for permission.
