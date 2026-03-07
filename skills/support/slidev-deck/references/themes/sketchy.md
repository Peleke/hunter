# Theme: Sketchy / Hand-Drawn

All SVGs in decks MUST follow the portfolio's dark-theme sketchy style. This is non-negotiable.

## Color Palette (Dark Slide Background)

| Role | Color | Hex |
|------|-------|-----|
| Slide background | GitHub dark | `#0d1117` |
| Primary text | Cream | `#eee0cc` |
| Accent (coral) | Warm coral | `#e88072` |
| Success (green) | Bright green | `#4ade80` |
| Danger (red) | Soft red | `#f87171` |
| Muted | Gray | `#888888` |
| Box fill | Translucent coral | `rgba(232,128,114,0.06)` |
| Highlighted box fill | Translucent coral (stronger) | `rgba(232,128,114,0.12)` |

## Font

```
font-family="'Caveat', 'Comic Sans MS', cursive"
```

- Titles: 20-24px, bold
- Labels: 14-16px
- Annotations: 11-13px
- Captions: 10-12px, muted (`#888888`), reduced opacity

## Sketchy Primitives

**Lines**: Never use `<line>`. Use `<path>` with cubic beziers and 1-3px jitter on control points.

```xml
<path d="M 50,51 C 90,48 130,53 170,49 C 210,52 240,48 250,51"
      stroke="#eee0cc" stroke-width="2" fill="none" stroke-linecap="round"/>
```

**Rectangles**: Never use `<rect>` for content boxes. Build from 4-segment `<path>` with jittered corners.

```xml
<path d="M 132,68 C 230,65 430,71 562,67
         C 565,85 563,105 565,128
         C 430,131 230,127 132,131
         C 129,113 131,87 129,68"
      stroke="#e88072" stroke-width="2" fill="rgba(232,128,114,0.06)"
      stroke-linecap="round" stroke-linejoin="round"/>
```

**Circles**: Use bezier-approximated circles, not `<circle>`, except for numbered callouts.

**Numbered callouts**: Coral fill circle with white bold number text.

```xml
<circle cx="108" cy="82" r="14" fill="#e88072" stroke="none" opacity="0.9"/>
<text x="108" y="87" text-anchor="middle"
      font-family="'Caveat', 'Comic Sans MS', cursive"
      font-size="14" fill="white" font-weight="bold">1</text>
```

**Arrows**: Bezier path body + marker-end arrowhead (or inline chevron path).

```xml
<defs>
  <marker id="arrow" viewBox="0 0 12 10" refX="10" refY="5"
          markerWidth="8" markerHeight="6" orient="auto-start-reverse"
          fill="none" stroke="#eee0cc" stroke-width="1.5"
          stroke-linecap="round" stroke-linejoin="round">
    <path d="M 1,1 L 10,5 L 1,9"/>
  </marker>
</defs>
<path d="M 336,134 C 338,150 334,162 336,176"
      stroke="#eee0cc" stroke-width="2" fill="none"
      stroke-linecap="round" marker-end="url(#arrow)"/>
```

**Rotation**: Apply slight rotation transforms (0.3-1.2 degrees, alternating positive/negative) to element groups.

```xml
<g transform="rotate(-0.5, 336, 630)">
  <!-- content -->
</g>
```

**Filters**: Use roughen filter for organic texture, glow filter for accent elements.

```xml
<defs>
  <filter id="roughen" x="-2%" y="-2%" width="104%" height="104%">
    <feTurbulence type="turbulence" baseFrequency="0.03" numOctaves="2" result="noise" seed="5"/>
    <feDisplacementMap in="SourceGraphic" in2="noise" scale="1.2" xChannelSelector="R" yChannelSelector="G"/>
  </filter>
  <filter id="glow">
    <feGaussianBlur stdDeviation="3" result="blur"/>
    <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
  </filter>
</defs>
```

**Dashed connectors**: Use `stroke-dasharray="4 4"` or `stroke-dasharray="8 5"` for secondary/feedback paths.

**Underlines**: Sketchy path under headings, same color as heading, reduced opacity.

```xml
<path d="M 172,46 C 240,48 360,44 500,47"
      stroke="#e88072" stroke-width="1.5" fill="none"
      stroke-linecap="round" opacity="0.6"/>
```

## ViewBox Sizing

All SVGs use `viewBox` with no fixed `width`/`height` (responsive). Standard slide SVG viewBox: `0 0 672 400` (fits 16:9 slides). Adjust height as needed but keep width at 672 for consistency.

## SVG Checklist (per SVG)

- [ ] No fixed `width`/`height` on root `<svg>` -- only `viewBox`
- [ ] No clean `<rect>` for content boxes -- use sketchy `<path>`
- [ ] No clean `<line>` for visible strokes -- use sketchy `<path>`
- [ ] Uses `Caveat` font family with fallback stack
- [ ] Slight rotation on element groups (varied, not uniform)
- [ ] Colors from the dark palette only (`#eee0cc`, `#e88072`, `#4ade80`, `#f87171`, `#888888`)
- [ ] No background rect (transparent -- sits on dark slide background)
- [ ] Arrows use marker-end or inline chevron paths
- [ ] Roughen filter on organic elements
- [ ] Self-contained: no external dependencies

## sli.dev Theme Overrides

```html
<style>
@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@400;700&display=swap');

:root {
  --slidev-theme-primary: #e88072;
  --slidev-theme-background: #0d1117;
}

.slidev-layout {
  background: #0d1117 !important;
  color: #e6edf3;
}

h1, h2, h3 {
  font-family: 'Caveat', 'Comic Sans MS', cursive !important;
  color: #eee0cc !important;
}

h1 { font-size: 2.5rem !important; }

p, li, td {
  color: #e6edf3;
  font-size: 1.1rem;
  line-height: 1.6;
}

strong { color: #e88072; }

.coral { color: #e88072; }
.green { color: #4ade80; }
.red { color: #f87171; }
.muted { color: #888888; }

blockquote {
  border-left: 3px solid #e88072;
  padding-left: 1rem;
  color: #888888;
  font-style: italic;
}

code {
  background: rgba(232,128,114,0.1);
  color: #e88072;
  padding: 0.1em 0.3em;
  border-radius: 3px;
}

table { width: 100%; }

th {
  color: #e88072;
  border-bottom: 2px solid rgba(232,128,114,0.3);
}

td {
  border-bottom: 1px solid rgba(238,224,204,0.1);
}

.svg-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 672px;
  margin: 0 auto;
}

.svg-container svg { width: 100%; height: auto; }

.stat-big {
  font-family: 'Caveat', cursive;
  font-size: 4rem;
  color: #e88072;
  font-weight: bold;
  line-height: 1;
}

.stat-label {
  color: #888888;
  font-size: 0.9rem;
}
</style>
```
