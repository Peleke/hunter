---
name: design-pass
description: "Apply aesthetic upgrades to visual assets (Slidev decks, landing pages, one-pagers). Configurable upgrade tiers from safe CSS additions to full animated polish."
metadata:
  openclaw:
    emoji: "\U0001F3A8"
---

# Design Pass

## Overview

Takes an existing visual asset and applies aesthetic CSS/SVG upgrades from a tiered catalog of design techniques. Non-destructive — adds to existing styles, does not restructure content or change copy.

Operates on three asset types:
- **Slidev deck**: `slides.md` + `style.css` + `public/*.svg` + Vue components
- **Landing page**: `index.html` (single-file or multi-file)
- **One-pager**: HTML for PDF export

## Configuration

### Via PipelineEnvelope

```json
{
  "skill": "design-pass",
  "input_refs": ["pitch-ambient-content-engine-2026-03-06"],
  "output": {
    "target": "slidev-deck",
    "tier": "high",
    "techniques": [],
    "brand": {}
  }
}
```

### Standalone (no envelope)

User says: "Run a design pass on the deck at launchpad/deck/"
Agent asks:
1. **Target type**: deck / landing page / one-pager? (auto-detect if possible)
2. **Tier**: high (safe), medium (moderate), full (everything), or custom (pick techniques)?
3. **Brand**: use existing brand config or skip?

### Tier Definitions

| Tier | Techniques | Token Budget | Risk |
|------|-----------|--------------|------|
| **high** | 1B, 1C, 4A, 4D, 4E | ~2K CSS | Zero — pure CSS additions |
| **medium** | high + 2A, 3A, 3B, 3C, 3F, 5C | ~4K CSS + minor HTML | Low — wrapper elements |
| **full** | medium + 1A, 1E, 1F, 2B, 3E, 4B, 4C, 5A, 5B | ~8K CSS + HTML + SVG | Medium — animations, browser caveats |
| **custom** | user-selected technique IDs | varies | varies |

### Technique Selection (custom tier)

User provides technique IDs from the catalog:
```json
{ "techniques": ["1B", "1C", "3A", "4D"] }
```

Or by name:
```json
{ "techniques": ["split-color-headings", "animated-underline", "dot-grid-bg", "blur-to-sharp"] }
```

---

## Workflow

### Phase 0: Inventory

1. Read the target asset files
2. Identify asset type (deck/landing/one-pager)
3. Read existing CSS — catalog what's already applied
4. Read `references/_design-conventions.md` for technique definitions
5. Determine which techniques from the requested tier are not yet present
6. Present the plan to the user: "I'll apply X techniques. Y are already present. Skip Z (conflicts)."

### Phase 1: Typography & Headings

Apply heading treatments first — they have the widest visual impact.

**Order**: split-color (1B) → animated underline (1C) → gradient text (1A) → glow (1E) → shimmer (1F)

For each:
1. Read technique CSS from `_design-conventions.md`
2. Adapt colors to the project's theme tokens (read from existing `style.css` or `:root` vars)
3. Append to `style.css` under a `/* Design Pass: Typography */` comment block
4. For split-color headings: edit Markdown headings to wrap first word in `<span class="teal">`

### Phase 2: Animations & Transitions

Apply reveal animations and micro-interactions.

**Order**: staggered v-click (4A) → blur-to-sharp (4D) → blockquote slide-in (4E) → v-motion (4B) → custom transitions (4C)

1. Append CSS under `/* Design Pass: Animations */`
2. For v-motion: edit slide HTML to add `v-motion` directives (medium/full only)
3. For custom transitions: add to `style.css` and update slide frontmatter `transition:` values

### Phase 3: Backgrounds & Layout

Apply cover slide treatments and layout upgrades.

**Order**: dot grid (3A) → radial halo (3B) → glassmorphism (3C) → animated grid (3D) → gradient border (3E) → metric cards (5A) → timeline (5B)

1. For cover backgrounds: add CSS classes and update cover slide `class:` in frontmatter
2. For glassmorphism: wrap cover content in `<div class="glass-card">`
3. For layout patterns: restructure specific slides' HTML (user approval required)

### Phase 4: SVG Polish (full tier only)

Apply SVG-specific enhancements.

1. Add `stroke-dasharray` + `stroke-dashoffset` animations to pipeline SVGs
2. Add staggered `opacity` animations to SVG text elements
3. Ensure all SVG animations use `forwards` fill mode

### Phase 5: Verify

1. List all changes made (file, technique, lines added)
2. Note skipped techniques with reasons
3. Note browser caveats (e.g., `@property` not in Firefox)
4. If brand mode: verify brand colors preserved throughout

---

## Asset-Specific Adapters

### Slidev Deck

| File | What gets modified |
|------|-------------------|
| `style.css` | All CSS additions (typography, animations, backgrounds) |
| `slides.md` | Heading span wraps, `class:` frontmatter, `v-motion` directives, cover slide HTML |
| `public/*.svg` | SVG animation attributes (full tier) |
| `uno.config.ts` | Custom animation keyframes, shortcuts (if UnoCSS is configured) |

**Slidev-specific rules:**
- All CSS overrides use `.slidev-layout` parent selector + `!important`
- `v-click` animations defined via `.slidev-vclick-hidden` / `.slidev-vclick-current`
- Use `<object>` for interactive SVGs, `<img>` for static
- Transitions defined in slide frontmatter, CSS in `style.css`

### Landing Page

| File | What gets modified |
|------|-------------------|
| `index.html` | `<style>` block additions, wrapper elements, class additions |

**Landing-specific rules:**
- CSS goes in `<style>` block (no external stylesheet assumed)
- Use CSS custom properties for theme tokens
- Glassmorphism + gradient borders are especially effective here

### One-Pager

| File | What gets modified |
|------|-------------------|
| HTML source | `<style>` additions, layout class changes |

**One-pager-specific rules:**
- Must work in print/PDF — no animations (skip Phase 2 and 4)
- Focus on typography (Phase 1) and layout (Phase 3)
- Glassmorphism does NOT render in PDF — skip 3C
- Gradient text does NOT render in PDF — use solid colors

---

## Theme Token Resolution

The design pass reads the project's existing theme and adapts technique colors:

```
1. Check :root CSS custom properties (e.g., --slidev-theme-primary)
2. Check style.css for hardcoded color values
3. Check brand metadata in PipelineEnvelope
4. Fall back to defaults: teal=#4DCFC9, cream=#eee0cc, bg=#0d1117, muted=#8b949e
```

**Never hardcode colors from the technique catalog.** Always resolve against the project's theme first.

---

## Quality Checklist

### All Tiers
- [ ] No existing styles overwritten (additive only — append, don't replace)
- [ ] All new CSS under labeled comment blocks (`/* Design Pass: ... */`)
- [ ] Brand colors preserved (if brand metadata present)
- [ ] Theme tokens resolved from project, not hardcoded

### Animation Tiers (medium, full)
- [ ] All animations use `forwards` fill mode
- [ ] No infinite loops unless intentional (only grid-fade)
- [ ] Transition timing uses project's cubic-bezier (default: `0.22, 1, 0.36, 1`)
- [ ] Neon/glow limited to 2-3 shadow layers max
- [ ] `@property` usage noted as Chromium/Safari only

### SVG Tier (full)
- [ ] SVG `viewBox` not broken by additions
- [ ] `stroke-dasharray` values match actual path lengths
- [ ] Stagger delays don't exceed 2s total
- [ ] No SVG animations on PDF-targeted assets

### One-Pager Specific
- [ ] No animations applied (print target)
- [ ] No glassmorphism (backdrop-filter doesn't print)
- [ ] No gradient text (background-clip doesn't print)
- [ ] Layout changes verified in print preview
