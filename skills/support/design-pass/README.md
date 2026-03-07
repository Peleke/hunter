<div align="center">

# 🎨 Design Pass

### Tiered aesthetic upgrades for decks, pages, and one-pagers

[![Tier](https://img.shields.io/badge/Tier-Output-4DCFC9?style=for-the-badge)]()
[![Target](https://img.shields.io/badge/Target-CSS%2FSVG-e88072?style=for-the-badge)]()

**Non-destructive visual polish. Additive only. Never restructure content.**

[Usage](#usage) · [Input/Output](#input--output) · [Tiers](#tiers) · [Quality Checklist](#quality-checklist)

---

</div>

## Overview

Design Pass takes an existing visual asset -- a Slidev deck, a landing page, or a one-pager -- and applies aesthetic CSS/SVG upgrades from a tiered catalog of techniques. It reads the project's existing theme tokens, adapts colors, and appends styles under labeled comment blocks. No content changes. No restructuring. Pure visual enhancement.

Three asset types supported:
- **Slidev deck**: `slides.md` + `style.css` + `public/*.svg` + Vue components
- **Landing page**: `index.html` (inline `<style>` block)
- **One-pager**: HTML for PDF export (print-safe techniques only)

## Tiers

| Tier | Techniques | Token Budget | Risk |
|---|---|---|---|
| **high** | Split-color headings, animated underline, v-click stagger, blur-to-sharp, blockquote slide-in | ~2K CSS | Zero -- pure CSS additions |
| **medium** | high + dot grid bg, radial halo, glassmorphism, metric cards, timeline | ~4K CSS + minor HTML | Low -- wrapper elements |
| **full** | medium + gradient text, glow, shimmer, v-motion, custom transitions, SVG animations | ~8K CSS + HTML + SVG | Medium -- animations, browser caveats |
| **custom** | User-selected technique IDs from the catalog | Varies | Varies |

### Technique Catalog

**Typography & Headings**
| ID | Name | Tier | Description |
|---|---|---|---|
| 1A | gradient-text | full | CSS gradient on heading text |
| 1B | split-color | high | First word in accent color via `<span>` |
| 1C | animated-underline | high | CSS underline animation on hover |
| 1E | glow | full | Text shadow glow effect |
| 1F | shimmer | full | Animated shimmer across text |

**Animations & Transitions**
| ID | Name | Tier | Description |
|---|---|---|---|
| 4A | staggered-v-click | high | Staggered reveal for v-click elements |
| 4B | v-motion | full | Vue motion directives on slide elements |
| 4C | custom-transitions | full | Slide-level transition overrides |
| 4D | blur-to-sharp | high | Elements blur-in on reveal |
| 4E | blockquote-slide-in | high | Blockquotes slide in from the left |

**Backgrounds & Layout**
| ID | Name | Tier | Description |
|---|---|---|---|
| 3A | dot-grid-bg | medium | Subtle dot grid background pattern |
| 3B | radial-halo | medium | Radial gradient halo behind cover content |
| 3C | glassmorphism | medium | Frosted glass card on cover slide |
| 3E | gradient-border | full | Animated gradient border on cards |
| 5A | metric-cards | medium | Stat cards with large numbers |
| 5B | timeline | medium | Vertical timeline layout component |

## Input

| Field | Description | Required |
|---|---|---|
| **Target path** | Path to the asset (deck directory, HTML file) | Yes |
| **Tier** | high / medium / full / custom | Yes |
| **Techniques** | Specific technique IDs (custom tier only) | If custom |
| **Brand** | Brand color overrides | Optional |

Or via `PipelineEnvelope`:

```json
{
  "skill": "design-pass",
  "output": {
    "target": "slidev-deck",
    "tier": "high",
    "techniques": [],
    "brand": {}
  }
}
```

## Output

Modified files depend on asset type:

| Asset | Files Modified |
|---|---|
| **Slidev deck** | `style.css`, `slides.md` (class/frontmatter), `public/*.svg` (full tier) |
| **Landing page** | `index.html` (`<style>` additions, wrapper elements) |
| **One-pager** | HTML source (`<style>` additions -- no animations, no glassmorphism) |

## Workflow

```
Target Asset + Tier Selection
    │
    ├── Phase 0: Inventory ─── read existing CSS, identify what is already applied
    │
    ├── Phase 1: Typography ─── split-color → underline → gradient → glow → shimmer
    │
    ├── Phase 2: Animations ─── v-click stagger → blur-to-sharp → blockquote → v-motion
    │
    ├── Phase 3: Backgrounds ─── dot grid → halo → glassmorphism → gradient border
    │
    ├── Phase 4: SVG Polish ─── stroke animations, staggered opacity (full tier only)
    │
    └── Phase 5: Verify ─── list changes, note skips, check browser caveats
```

### Theme Token Resolution

```
1. Check :root CSS custom properties (--slidev-theme-primary)
2. Check style.css for hardcoded values
3. Check PipelineEnvelope brand metadata
4. Fall back to defaults: teal=#4DCFC9, cream=#eee0cc, bg=#0d1117
```

Never hardcode catalog colors. Always resolve against the project theme first.

### One-Pager Restrictions

Print/PDF target means no animations, no glassmorphism (`backdrop-filter` does not print), no gradient text (`background-clip` does not print). Focus on typography and layout only.

## Usage

```
"Run a design pass on the deck at launchpad/deck/"
"Apply high-tier polish to the landing page"
"Design pass on this one-pager, full tier"
"Apply techniques 1B, 1C, 3A, 4D to the deck"
```

## Quality Checklist

- [ ] No existing styles overwritten (additive only)
- [ ] All new CSS under labeled comment blocks (`/* Design Pass: ... */`)
- [ ] Brand colors preserved if brand metadata present
- [ ] Theme tokens resolved from project, not hardcoded
- [ ] All animations use `forwards` fill mode
- [ ] No infinite loops unless intentional
- [ ] `@property` usage noted as Chromium/Safari only
- [ ] SVG `viewBox` not broken
- [ ] One-pager: no animations, no glassmorphism, no gradient text

---

<div align="center">

**Polish what exists. Never break what works.**

[![Pipeline](https://img.shields.io/badge/Role-Output-4DCFC9?style=for-the-badge)]()

</div>
