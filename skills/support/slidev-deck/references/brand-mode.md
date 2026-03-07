# Brand Mode Reference

Complete templates and styling for the brand mode feature across visual skills (slidev-deck, landing-page, one-pager).

---

## global-bottom.vue Template (Slidev Deck)

This file goes in the deck root directory (same level as `slides.md`). Slidev automatically picks up `global-bottom.vue` and renders it at the bottom of every slide.

### Variant A: Image Logo + CONFIDENTIAL

Use this when the user provides an SVG or PNG logo file AND enables the CONFIDENTIAL watermark.

**Logo file setup**: Copy the user's logo to `public/` in the deck root (e.g., `public/logo.png`). Use `<img src="/logo.png" class="footer-logo-img" />`. The logo should be a PNG or SVG with a **transparent background** — this is required for correct compositing on the dark slide background. Opaque-background logos will render as bright rectangles. The `filter: brightness(1.2)` compensates for the low opacity by slightly lifting detail in the logo.

```vue
<template>
  <div class="brand-footer">
    <span class="brand-confidential">CONFIDENTIAL</span>
    <img src="/logo.png" class="footer-logo-img" alt="Brand logo" />
  </div>
</template>

<style scoped>
.brand-footer {
  position: fixed;
  bottom: 12px;
  left: 24px;
  right: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  pointer-events: none;
  z-index: 100;
}

.brand-confidential {
  font-size: 9px;
  letter-spacing: 0.18em;
  color: rgba(238, 224, 204, 0.12);
  text-transform: uppercase;
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}

.footer-logo-img {
  height: 18px;
  opacity: 0.18;
  filter: brightness(1.2);
  user-select: none;
}
</style>
```

### Variant B: Text Wordmark + CONFIDENTIAL

Use this when the user provides a brand name but no logo image file.

```vue
<template>
  <div class="brand-footer">
    <span class="brand-confidential">CONFIDENTIAL</span>
    <span class="brand-wordmark">{{ brandName }}</span>
  </div>
</template>

<script setup>
const brandName = 'BRAND_NAME_HERE'
</script>

<style scoped>
.brand-footer {
  position: fixed;
  bottom: 12px;
  left: 24px;
  right: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  pointer-events: none;
  z-index: 100;
}

.brand-confidential {
  font-size: 9px;
  letter-spacing: 0.18em;
  color: rgba(238, 224, 204, 0.12);
  text-transform: uppercase;
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}

.brand-wordmark {
  font-size: 10px;
  color: rgba(77, 207, 201, 0.18);
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}
</style>
```

### Variant C: Image Logo Only (No CONFIDENTIAL)

Use this when the user provides a logo but does NOT want the CONFIDENTIAL watermark. Same logo file requirements as Variant A (transparent background PNG or SVG).

```vue
<template>
  <div class="brand-footer">
    <span></span>
    <img src="/logo.png" class="footer-logo-img" alt="Brand logo" />
  </div>
</template>

<style scoped>
.brand-footer {
  position: fixed;
  bottom: 12px;
  left: 24px;
  right: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  pointer-events: none;
  z-index: 100;
}

.footer-logo-img {
  height: 18px;
  opacity: 0.18;
  filter: brightness(1.2);
  user-select: none;
}
</style>
```

### Variant D: Text Wordmark Only (No CONFIDENTIAL)

Use this when the user provides a brand name, no logo, and no CONFIDENTIAL watermark.

```vue
<template>
  <div class="brand-footer">
    <span></span>
    <span class="brand-wordmark">{{ brandName }}</span>
  </div>
</template>

<script setup>
const brandName = 'BRAND_NAME_HERE'
</script>

<style scoped>
.brand-footer {
  position: fixed;
  bottom: 12px;
  left: 24px;
  right: 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  pointer-events: none;
  z-index: 100;
}

.brand-wordmark {
  font-size: 10px;
  color: rgba(77, 207, 201, 0.18);
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}
</style>
```

### Logo File Placement

When the user provides a logo file path:

1. Create a `public/` directory in the deck root (if it does not exist)
2. Copy the logo file to `public/logo.png` (or `public/logo.svg`)
3. Reference it in the template as `src="/logo.png"` (or `src="/logo.svg"`)

Slidev serves the `public/` directory at the root path, so `/logo.png` resolves correctly.

**Logo requirements**: The logo MUST have a transparent background. PNG and SVG with transparency both work. Opaque-background logos will render as bright rectangles against the dark slide background. If the user provides a logo with an opaque background, ask them for a version with transparency before proceeding.

---

## CSS Reference: All Brand Styles

These are the canonical style values for brand elements across all visual skills. They are intentionally ghosted -- visible on close inspection, invisible during presentation or casual reading.

```css
/* CONFIDENTIAL watermark text */
.brand-confidential {
  font-size: 9px;
  letter-spacing: 0.18em;
  color: rgba(238, 224, 204, 0.12);
  text-transform: uppercase;
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}

/* Text wordmark (when no logo image is provided) */
.brand-wordmark {
  font-size: 10px;
  color: rgba(77, 207, 201, 0.18);
  font-family: 'Fira Code', 'Fira Mono', monospace;
  user-select: none;
}

/* Logo image (when SVG or PNG is provided — must have transparent background) */
.footer-logo-img {
  height: 18px;
  opacity: 0.18;
  filter: brightness(1.2);
  user-select: none;
}
```

---

## Landing Page Branded Footer HTML Template

Add this just above the closing `</body>` tag (or replace the existing `<footer>` inner content). Uses the same CSS custom properties defined in the landing page design system.

### With CONFIDENTIAL + Image Logo

```html
<div class="brand-bar">
  <span class="brand-confidential">CONFIDENTIAL</span>
  <img src="logo.svg" class="brand-bar-logo" alt="Brand logo" />
</div>

<style>
  .brand-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 24px;
    background: var(--bg, #0d1117);
    border-top: 1px solid var(--border, #30363d);
  }
  .brand-confidential {
    font-size: 9px;
    letter-spacing: 0.18em;
    color: rgba(238, 224, 204, 0.12);
    text-transform: uppercase;
    font-family: 'Fira Code', 'Fira Mono', monospace;
    user-select: none;
  }
  .brand-bar-logo {
    height: 18px;
    opacity: 0.18;
    filter: brightness(1.2);
    user-select: none;
  }
</style>
```

### With CONFIDENTIAL + Text Wordmark

```html
<div class="brand-bar">
  <span class="brand-confidential">CONFIDENTIAL</span>
  <span class="brand-wordmark">Brand Name</span>
</div>

<style>
  .brand-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 24px;
    background: var(--bg, #0d1117);
    border-top: 1px solid var(--border, #30363d);
  }
  .brand-confidential {
    font-size: 9px;
    letter-spacing: 0.18em;
    color: rgba(238, 224, 204, 0.12);
    text-transform: uppercase;
    font-family: 'Fira Code', 'Fira Mono', monospace;
    user-select: none;
  }
  .brand-wordmark {
    font-size: 10px;
    color: rgba(77, 207, 201, 0.18);
    font-family: 'Fira Code', 'Fira Mono', monospace;
    user-select: none;
  }
</style>
```

### Without CONFIDENTIAL (Logo Only or Wordmark Only)

Omit the `.brand-confidential` span. Use the same `.brand-bar` container with an empty `<span></span>` on the left to maintain the flex layout:

```html
<div class="brand-bar">
  <span></span>
  <img src="logo.svg" class="brand-bar-logo" alt="Brand logo" />
</div>
```

Or with text wordmark:

```html
<div class="brand-bar">
  <span></span>
  <span class="brand-wordmark">Brand Name</span>
</div>
```

---

## PipelineEnvelope Brand Metadata Example

When brand mode is active, the full PipelineEnvelope looks like this:

```json
{
  "skill": "slidev-deck",
  "version": "1.0",
  "session_id": "session-2026-03-06-001",
  "timestamp": "2026-03-06T14:30:00Z",
  "input_refs": ["devops-decision-kit-pitch-2026-03-01"],
  "metadata": {
    "brand": {
      "name": "Acme Corp",
      "logoPath": "/Users/peleke/Documents/brand-assets/acme-logo.svg",
      "confidential": true,
      "tagline": "Ship faster, break nothing"
    }
  },
  "output": {
    "deck_id": "devops-decision-kit-product-pitch-2026-03-06",
    "slides_path": "/Users/peleke/Documents/Projects/decks/devops-decision-kit-product-pitch/slides.md",
    "global_bottom_path": "/Users/peleke/Documents/Projects/decks/devops-decision-kit-product-pitch/global-bottom.vue",
    "slide_count": 16,
    "has_brand": true
  }
}
```

When brand mode is **not** active, the `metadata` field is omitted entirely:

```json
{
  "skill": "slidev-deck",
  "version": "1.0",
  "session_id": "session-2026-03-06-002",
  "timestamp": "2026-03-06T15:00:00Z",
  "input_refs": ["devops-decision-kit-pitch-2026-03-01"],
  "output": {
    "deck_id": "devops-decision-kit-product-pitch-2026-03-06",
    "slides_path": "/Users/peleke/Documents/Projects/decks/devops-decision-kit-product-pitch/slides.md",
    "slide_count": 16,
    "has_brand": false
  }
}
```

The same pattern applies to `landing-page` and `one-pager` skills -- only the `skill` field and `output` contents change.

---

## Decision Matrix: Which Variant to Use

| Has Logo? | Has CONFIDENTIAL? | Deck Variant | Landing Page |
|-----------|-------------------|-------------|-------------|
| Yes | Yes | Variant A | CONFIDENTIAL + Image Logo |
| No | Yes | Variant B | CONFIDENTIAL + Text Wordmark |
| Yes | No | Variant C | Logo Only |
| No | No | Variant D | Wordmark Only |
