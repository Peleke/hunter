# sli.dev Base Configuration

This file covers the sli.dev frontmatter and slide structure conventions that apply to ALL themes.

For the `<style>` block (fonts, colors, component classes), see the selected theme file in `themes/`.

## Deck Frontmatter

Every deck starts with this frontmatter block:

```md
---
theme: default
title: "DECK TITLE"
info: |
  DECK SUBTITLE OR DESCRIPTION
class: text-white
drawings:
  persist: false
transition: fade-out
mdc: true
---
```

## Slide Structure

- Slides are separated by `---` on their own line
- Layout directives go in each slide's frontmatter block
- Global `<style>` block goes on the FIRST slide only (after content)
- The `<style>` block content comes from the selected theme file

### Slide Frontmatter

```md
---
layout: center
---
```

Available layouts: `cover`, `center`, `two-cols`, `default`, `image-right`, `image-left`, `fact`, `quote`.

### Two-Column Layout

```md
---
layout: two-cols
---

# Left Column

Content here

::right::

# Right Column

Content here
```

### Speaker Notes

Every slide MUST have speaker notes as HTML comments at the end:

```md
# Slide Title

content

<!--
Speaker notes here. These appear in presenter view.
- Key talking point 1
- Key talking point 2
-->
```

### Running the Deck

```bash
npx slidev slides.md
```

No `package.json` required. For persistent setup:

```bash
npm init slidev@latest
# then replace slides.md with the generated file
```
