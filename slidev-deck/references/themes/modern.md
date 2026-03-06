# Theme: Modern Corporate

Clean, professional, Inter/Helvetica-flavored. Dark background retained. No sketchy primitives.

## Color Palette

| Role | Color | Hex |
|------|-------|-----|
| Slide background | GitHub dark | `#0d1117` |
| Primary text | Cool white | `#e6edf3` |
| Accent | Warm coral | `#e88072` |
| Success | Green | `#4ade80` |
| Danger | Red | `#f87171` |
| Muted | Gray | `#8b949e` |
| Surface | Elevated card | `rgba(255,255,255,0.04)` |
| Border | Subtle | `rgba(255,255,255,0.08)` |

## Typography

```
font-family: 'Inter', 'Helvetica Neue', Helvetica, Arial, sans-serif
```

- H1: 2.5rem, weight 700, letter-spacing -0.02em
- H2: 1.8rem, weight 600
- Body: 1.1rem, weight 400, line-height 1.6
- Labels: 0.85rem, weight 500, uppercase, letter-spacing 0.08em
- Stats: 3.5rem, weight 700, `#e88072`

## SVG Conventions

Use clean geometric shapes. No bezier jitter. No roughen filter.

**Rectangles**: Use `<rect>` with `rx="8"` for rounded corners.
```xml
<rect x="50" y="30" width="200" height="80" rx="8"
      fill="rgba(255,255,255,0.04)" stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
```

**Lines**: Use clean `<line>` or `<path>` without jitter.
```xml
<line x1="50" y1="200" x2="620" y2="200" stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
```

**Circles**: Use `<circle>` directly.
```xml
<circle cx="108" cy="82" r="16" fill="#e88072" stroke="none"/>
<text x="108" y="87" text-anchor="middle"
      font-family="'Inter', sans-serif"
      font-size="13" fill="white" font-weight="600">1</text>
```

**Arrows**: Clean marker-end, no organic feel.
```xml
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5"
          markerWidth="6" markerHeight="6" orient="auto"
          fill="#8b949e">
    <path d="M 0,0 L 10,5 L 0,10 z"/>
  </marker>
</defs>
<line x1="100" y1="150" x2="300" y2="150"
      stroke="#8b949e" stroke-width="1.5" marker-end="url(#arrow)"/>
```

**Cards**: Elevated surfaces with subtle border and background.
```xml
<rect x="40" y="60" width="260" height="120" rx="8"
      fill="rgba(255,255,255,0.04)" stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
```

**Accent elements**: Coral left-border accent bar on key cards.
```xml
<rect x="40" y="60" width="4" height="120" rx="2" fill="#e88072"/>
```

**No rotation transforms**. No roughen filter. No glow filter. Clean and sharp.

## ViewBox Sizing

Same as base: `viewBox="0 0 672 NNN"`, no fixed width/height. Width 672 for 16:9 consistency.

## SVG Checklist (Modern Theme)

- [ ] No fixed `width`/`height` on root `<svg>` -- only `viewBox`
- [ ] Uses Inter font family with sans-serif fallback
- [ ] Clean geometric shapes (`<rect>`, `<circle>`, `<line>`)
- [ ] Rounded corners (`rx="8"`) on all rectangles
- [ ] Colors from the dark palette only
- [ ] No background rect (transparent on dark slide)
- [ ] Subtle borders: `rgba(255,255,255,0.08)`
- [ ] Accent bar (4px coral `<rect>`) on key cards
- [ ] No rotation, no roughen filter, no glow

## sli.dev Theme Overrides

```html
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

:root {
  --slidev-theme-primary: #e88072;
  --slidev-theme-background: #0d1117;
}

.slidev-layout {
  background: #0d1117 !important;
  color: #e6edf3;
}

h1, h2, h3 {
  font-family: 'Inter', 'Helvetica Neue', sans-serif !important;
  color: #e6edf3 !important;
  letter-spacing: -0.02em;
}

h1 { font-size: 2.5rem !important; font-weight: 700 !important; }
h2 { font-size: 1.8rem !important; font-weight: 600 !important; }

p, li, td {
  color: #e6edf3;
  font-family: 'Inter', sans-serif;
  font-size: 1.1rem;
  line-height: 1.6;
}

strong { color: #e88072; }

.coral { color: #e88072; }
.green { color: #4ade80; }
.red { color: #f87171; }
.muted { color: #8b949e; }

blockquote {
  border-left: 3px solid #e88072;
  padding-left: 1rem;
  color: #8b949e;
  font-style: normal;
}

code {
  background: rgba(232,128,114,0.08);
  color: #e88072;
  padding: 0.15em 0.4em;
  border-radius: 4px;
  font-size: 0.9em;
}

table { width: 100%; border-collapse: collapse; }

th {
  color: #e88072;
  border-bottom: 2px solid rgba(232,128,114,0.2);
  font-weight: 600;
  text-align: left;
  padding: 0.5rem 0;
}

td {
  border-bottom: 1px solid rgba(255,255,255,0.06);
  padding: 0.5rem 0;
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
  font-family: 'Inter', sans-serif;
  font-size: 3.5rem;
  color: #e88072;
  font-weight: 700;
  line-height: 1;
  letter-spacing: -0.03em;
}

.stat-label {
  color: #8b949e;
  font-size: 0.85rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}
</style>
```
