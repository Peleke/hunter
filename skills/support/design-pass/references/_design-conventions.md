---
name: design-conventions
description: "Catalog of aesthetic design techniques for dark-theme visual assets. Referenced by the design-pass skill."
---

# Design Conventions — Technique Catalog

## Theme Tokens (defaults)

Resolve from project first. These are fallbacks only.

| Token | Default | Usage |
|-------|---------|-------|
| `--accent` | `#4DCFC9` | Primary accent (teal) |
| `--text-warm` | `#eee0cc` | Headings, emphasis |
| `--text-body` | `#e6edf3` | Body text |
| `--text-muted` | `#8b949e` | Secondary text |
| `--bg` | `#0d1117` | Background |
| `--danger` | `#f87171` | Red accent |
| `--success` | `#4ade80` | Green accent |
| `--warn` | `#facc15` | Yellow accent |
| `--font-sans` | `'Inter', system-ui, sans-serif` | Body + headings |
| `--font-mono` | `'Fira Code', monospace` | Code, badges, pipelines |
| `--ease` | `cubic-bezier(0.22, 1, 0.36, 1)` | Standard easing |

---

## Category 1: Typography & Headings

### 1A — Gradient Text

**Tier**: full | **Risk**: low | **Target**: deck, landing | **Not for**: one-pager (print)

Heading text fills with a teal-to-cream gradient.

```css
h2.text-gradient {
  background: linear-gradient(135deg, var(--accent) 0%, var(--text-warm) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}
```

**Variant — animated shimmer on slide enter:**

```css
@keyframes gradient-shift {
  0%   { background-position: 0% 50%; }
  100% { background-position: 100% 50%; }
}

h2.shimmer {
  background: linear-gradient(90deg, var(--accent), var(--text-warm), var(--accent));
  background-size: 200% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: gradient-shift 3s ease-in-out forwards;
}
```

### 1B — Split-Color Headings

**Tier**: high | **Risk**: zero | **Target**: all

First word in accent color, rest in warm cream. The simplest high-impact change.

**Markdown** (Slidev):
```md
## <span class="teal">Evidence</span> Wall
```

**CSS** (already handled by `.teal` class in most projects):
```css
h2 .teal { color: var(--accent); }
```

**Application rule**: wrap the most semantically meaningful word, not always the first. "Four Forces" → `<span class="teal">Four</span> Forces`. "Kill Criteria" → `<span class="teal">Kill</span> Criteria`.

### 1C — Animated Underline

**Tier**: high | **Risk**: zero | **Target**: deck, landing

Gradient underline grows from left on slide/page enter.

```css
h2 {
  position: relative;
  display: inline-block;
}

h2::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, var(--accent), transparent);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.6s var(--ease);
}

/* Slidev: trigger on slide active */
.slidev-page h2::after {
  transform: scaleX(1);
}

/* Landing page: trigger on scroll-into-view or load */
h2.visible::after {
  transform: scaleX(1);
}
```

### 1D — Letterpress / Emboss

**Tier**: full | **Risk**: low | **Target**: all

Subtle 3D pressed-into-surface effect.

```css
h2.engraved {
  color: rgba(238, 224, 204, 0.7);
  text-shadow: 0 1px 1px rgba(255, 255, 255, 0.08);
}
```

### 1E — Neon Glow

**Tier**: full | **Risk**: medium (easy to overdo) | **Target**: deck, landing

**MAX 3 shadow layers.** More looks cheap.

```css
h2.glow {
  color: var(--accent);
  text-shadow:
    0 0 7px rgba(77, 207, 201, 0.5),
    0 0 20px rgba(77, 207, 201, 0.3),
    0 0 40px rgba(77, 207, 201, 0.15);
}
```

### 1F — Shimmer Sweep

**Tier**: full | **Risk**: low | **Target**: deck, landing

Highlight sweeps across text once on enter. Use only on 1-2 key headings per deck.

```css
@keyframes shimmer-sweep {
  0%   { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

h2.text-shimmer {
  background: linear-gradient(90deg, var(--text-warm) 0%, var(--text-warm) 40%, var(--accent) 50%, var(--text-warm) 60%, var(--text-warm) 100%);
  background-size: 200% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: shimmer-sweep 2s ease-in-out 0.3s forwards;
}
```

---

## Category 2: Subheadings & Metadata

### 2A — Monospace Pill Badge

**Tier**: medium | **Risk**: zero | **Target**: all

Turn dates and version strings into styled badges.

```css
.date-badge {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: 0.85rem;
  color: var(--accent);
  background: rgba(77, 207, 201, 0.10);
  border: 1px solid rgba(77, 207, 201, 0.25);
  border-radius: 999px;
  padding: 2px 12px;
  margin-left: 8px;
  vertical-align: middle;
  letter-spacing: 0.04em;
}
```

### 2B — SVG Line Drawing Animation

**Tier**: full | **Risk**: low | **Target**: deck, landing | **Not for**: one-pager

Pipeline line draws itself left-to-right, stages appear with staggered delays.

```css
@keyframes draw-line {
  to { stroke-dashoffset: 0; }
}

@keyframes node-appear {
  0%   { opacity: 0; transform: scale(0.8); }
  100% { opacity: 1; transform: scale(1); }
}

.pipeline-line {
  stroke-dasharray: var(--line-length);
  stroke-dashoffset: var(--line-length);
  animation: draw-line 2s ease-in-out forwards;
}

.pipeline-node {
  opacity: 0;
  animation: node-appear 0.4s ease-out forwards;
}

/* Stagger: each node delayed by 0.3s */
.pipeline-node:nth-child(1) { animation-delay: 0.0s; }
.pipeline-node:nth-child(2) { animation-delay: 0.3s; }
.pipeline-node:nth-child(3) { animation-delay: 0.6s; }
.pipeline-node:nth-child(4) { animation-delay: 0.9s; }
.pipeline-node:nth-child(5) { animation-delay: 1.2s; }
.pipeline-node:nth-child(6) { animation-delay: 1.5s; }
.pipeline-node:nth-child(7) { animation-delay: 1.8s; }
```

**Implementation**: Add `class="pipeline-line"` and `class="pipeline-node"` to existing SVG elements. Set `--line-length` via JS or hardcode from path length.

---

## Category 3: Cover Slide & Backgrounds

### 3A — Dot Grid Background

**Tier**: medium | **Risk**: zero | **Target**: deck, landing

Subtle grid of dots that slowly pulses. CSS only.

```css
.cover-bg::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: radial-gradient(circle, rgba(77, 207, 201, 0.12) 1px, transparent 1px);
  background-size: 32px 32px;
  animation: grid-fade 4s ease-in-out infinite alternate;
}

@keyframes grid-fade {
  0%   { opacity: 0.3; }
  100% { opacity: 0.7; }
}
```

### 3B — Radial Halo

**Tier**: medium | **Risk**: zero | **Target**: deck, landing

Soft teal glow behind the title area.

```css
.cover-bg::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse 60% 50% at 50% 45%, rgba(77, 207, 201, 0.06) 0%, transparent 70%);
  pointer-events: none;
}
```

### 3C — Glassmorphism Card

**Tier**: medium | **Risk**: low | **Target**: deck, landing | **Not for**: one-pager (print)

Frosted-glass card.

```css
.glass-card {
  background: rgba(13, 17, 23, 0.6);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(77, 207, 201, 0.15);
  border-radius: 16px;
  padding: 40px 56px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3), inset 0 1px 0 rgba(238, 224, 204, 0.05);
}
```

### 3D — Animated Grid Lines

**Tier**: full | **Risk**: low | **Target**: deck, landing

Faint grid lines that slowly scroll. Masked to fade at edges.

```css
@keyframes grid-scroll {
  0%   { background-position: 0 0; }
  100% { background-position: 0 32px; }
}

.animated-grid::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(77, 207, 201, 0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(77, 207, 201, 0.04) 1px, transparent 1px);
  background-size: 32px 32px;
  animation: grid-scroll 8s linear infinite;
  mask-image: radial-gradient(ellipse 70% 70% at 50% 50%, black 30%, transparent 80%);
  -webkit-mask-image: radial-gradient(ellipse 70% 70% at 50% 50%, black 30%, transparent 80%);
}
```

### 3E — Gradient Border Glow Card

**Tier**: full | **Risk**: medium | **Target**: deck, landing | **Browser**: Chromium + Safari 16.4+ only

Animated conic gradient border that rotates once and settles.

```css
@property --angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

@keyframes border-rotate {
  to { --angle: 360deg; }
}

.glow-border-card {
  position: relative;
  padding: 40px 56px;
  border-radius: 16px;
  background: var(--bg);
  isolation: isolate;
}

.glow-border-card::before {
  content: '';
  position: absolute;
  inset: -2px;
  border-radius: inherit;
  background: conic-gradient(from var(--angle), transparent 0%, var(--accent) 25%, transparent 50%, var(--text-warm) 75%, transparent 100%);
  z-index: -1;
  animation: border-rotate 3s ease-in-out forwards;
}

.glow-border-card::after {
  content: '';
  position: absolute;
  inset: 2px;
  border-radius: inherit;
  background: var(--bg);
  z-index: -1;
}
```

### 3F — Circuit Pattern Background

**Tier**: medium | **Risk**: zero | **Target**: deck, landing

Tileable SVG circuit pattern with L-shaped paths and node dots at intersections. Applied as a pseudo-element background on slide layouts. Cover slide is EXCLUDED (gets dot grid + halo instead).

**SVG**: Create a tileable `circuit-pattern.svg` in `public/` with teal strokes at ~0.07 opacity and dot fills at ~0.10 opacity.

```css
.slidev-layout::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url('/circuit-pattern.svg');
  background-repeat: repeat;
  opacity: 0.6;
  pointer-events: none;
  z-index: 0;
}

/* Content must sit above the pattern */
.slidev-layout > * {
  position: relative;
  z-index: 1;
}

/* Exclude cover slide — it uses dot grid + halo instead */
.slidev-layout.cover::before {
  background-image: none;
}
```

---

## Category 4: Animations & Transitions

### 4A — Staggered v-click Reveals

**Tier**: high | **Risk**: zero | **Target**: deck

```css
.slidev-vclick-target:nth-child(1) { transition-delay: 0.0s; }
.slidev-vclick-target:nth-child(2) { transition-delay: 0.1s; }
.slidev-vclick-target:nth-child(3) { transition-delay: 0.15s; }
.slidev-vclick-target:nth-child(4) { transition-delay: 0.2s; }
```

### 4B — v-motion Directive

**Tier**: full | **Risk**: low | **Target**: deck

Per-element Spring physics via `@vueuse/motion` (bundled with Slidev).

```html
<div
  v-motion
  :initial="{ opacity: 0, y: 40, scale: 0.95 }"
  :enter="{ opacity: 1, y: 0, scale: 1, transition: { duration: 600, delay: 200 } }"
>
  <h2>Evidence Wall</h2>
</div>
```

### 4C — Teal Wipe Transition

**Tier**: full | **Risk**: low | **Target**: deck

Slide wipes from left to right via `clip-path`.

```css
.teal-wipe-enter-active,
.teal-wipe-leave-active {
  transition: all 0.6s var(--ease);
}

.teal-wipe-enter-from {
  opacity: 0;
  clip-path: inset(0 100% 0 0);
}

.teal-wipe-enter-to,
.teal-wipe-leave-from {
  opacity: 1;
  clip-path: inset(0 0 0 0);
}

.teal-wipe-leave-to {
  opacity: 0;
  clip-path: inset(0 0 0 100%);
}
```

**Usage**: Set `transition: teal-wipe` in slide frontmatter.

### 4D — Scale + Blur Entry

**Tier**: high | **Risk**: zero | **Target**: deck

Images and SVGs blur-to-sharp on reveal.

```css
.slidev-vclick-hidden img,
.slidev-vclick-hidden object {
  opacity: 0 !important;
  transform: scale(0.94);
  filter: blur(8px);
}

.slidev-vclick-current img,
.slidev-vclick-prior img,
.slidev-vclick-current object,
.slidev-vclick-prior object {
  opacity: 1;
  transform: scale(1);
  filter: blur(0);
  transition: all 0.5s var(--ease) !important;
}
```

### 4E — Blockquote Slide-In

**Tier**: high | **Risk**: zero | **Target**: deck

Quotes slide in from the left.

```css
.slidev-vclick-hidden blockquote {
  opacity: 0 !important;
  transform: translateX(-30px);
}

.slidev-vclick-current blockquote,
.slidev-vclick-prior blockquote {
  opacity: 1;
  transform: translateX(0);
  transition: all 0.5s var(--ease) !important;
}
```

---

## Category 5: Layout Patterns

### 5A — Metric Card Grid

**Tier**: full | **Risk**: low | **Target**: deck, landing

Replaces tables for metric-heavy slides.

```css
.metric-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 16px;
  max-width: 720px;
  margin: 24px auto;
}

.metric-card {
  background: rgba(77, 207, 201, 0.04);
  border: 1px solid rgba(77, 207, 201, 0.12);
  border-radius: 12px;
  padding: 20px 16px;
  text-align: center;
  display: flex;
  flex-direction: column;
  gap: 8px;
  transition: border-color 0.3s ease, background 0.3s ease;
}

.metric-card:hover {
  border-color: rgba(77, 207, 201, 0.3);
  background: rgba(77, 207, 201, 0.08);
}

.metric-value {
  font-size: 2rem;
  font-weight: 800;
  line-height: 1;
  color: var(--accent);
}

.metric-label {
  font-size: 0.85rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.06em;
}
```

### 5B — Vertical Timeline

**Tier**: full | **Risk**: low | **Target**: deck, landing

Replaces table-based timelines.

```css
.timeline {
  position: relative;
  padding-left: 32px;
  max-width: 600px;
  margin: 16px auto;
}

.timeline::before {
  content: '';
  position: absolute;
  left: 11px;
  top: 4px;
  bottom: 4px;
  width: 2px;
  background: linear-gradient(to bottom, var(--accent), rgba(77, 207, 201, 0.15));
}

.timeline-marker {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--bg);
  border: 2px solid rgba(77, 207, 201, 0.4);
  position: absolute;
  left: -26px;
  margin-top: 4px;
}

.timeline-marker.active {
  background: var(--accent);
  border-color: var(--accent);
  box-shadow: 0 0 8px rgba(77, 207, 201, 0.4);
}

.timeline-date {
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--accent);
}

.timeline-title {
  color: var(--text-warm);
  font-weight: 600;
}

.timeline-desc {
  color: var(--text-muted);
  font-size: 0.85rem;
}
```

### 5C — Card-Like Table Rows

**Tier**: medium | **Risk**: zero | **Target**: all

Replace flat table rows with card-like hover effects. Compatible with all targets including one-pager (no animations).

```css
table {
  border-collapse: separate;
  border-spacing: 0 4px;
  width: 100%;
}

table thead th {
  text-transform: uppercase;
  font-size: 0.75rem;
  letter-spacing: 0.08em;
  color: var(--text-muted);
  border-bottom: 1px solid rgba(77, 207, 201, 0.2);
  padding: 8px 12px;
}

table tbody tr {
  background: rgba(77, 207, 201, 0.02);
  transition: background 0.2s ease;
}

table tbody tr:hover {
  background: rgba(77, 207, 201, 0.06);
}

table tbody td {
  padding: 10px 12px;
}

table tbody td:first-child {
  border-radius: 8px 0 0 8px;
  border-left: 2px solid transparent;
  transition: border-color 0.2s ease;
}

table tbody td:last-child {
  border-radius: 0 8px 8px 0;
}

table tbody tr:hover td:first-child {
  border-left-color: var(--accent);
}
```

---

## Technique Index

Quick reference for tier selection.

| ID | Name | Tier | Risk | Deck | Landing | One-Pager |
|----|------|------|------|------|---------|-----------|
| 1A | Gradient text | full | low | yes | yes | NO |
| 1B | Split-color headings | high | zero | yes | yes | yes |
| 1C | Animated underline | high | zero | yes | yes | NO |
| 1D | Letterpress/emboss | full | low | yes | yes | yes |
| 1E | Neon glow | full | medium | yes | yes | NO |
| 1F | Shimmer sweep | full | low | yes | yes | NO |
| 2A | Monospace pill badge | medium | zero | yes | yes | yes |
| 2B | SVG line drawing | full | low | yes | yes | NO |
| 3A | Dot grid background | medium | zero | yes | yes | NO |
| 3B | Radial halo | medium | zero | yes | yes | NO |
| 3C | Glassmorphism card | medium | low | yes | yes | NO |
| 3D | Animated grid lines | full | low | yes | yes | NO |
| 3E | Gradient border glow | full | medium | yes | yes | NO |
| 4A | Staggered v-click | high | zero | yes | no | no |
| 4B | v-motion directive | full | low | yes | no | no |
| 4C | Teal wipe transition | full | low | yes | no | no |
| 4D | Blur-to-sharp entry | high | zero | yes | no | no |
| 4E | Blockquote slide-in | high | zero | yes | no | no |
| 5A | Metric card grid | full | low | yes | yes | yes |
| 5B | Vertical timeline | full | low | yes | yes | yes |
| 3F | Circuit pattern bg | medium | zero | yes | yes | NO |
| 5C | Card-like table rows | medium | zero | yes | yes | yes |
