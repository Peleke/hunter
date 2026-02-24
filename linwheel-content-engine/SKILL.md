---
name: linwheel-content-engine
description: >
  Drive the LinWheel LinkedIn content engine via first-class tools. Analyze source
  content, reshape it into angle-specific LinkedIn posts, refine drafts with LLM editing,
  attach hero images and carousels, manage voice profiles, approve posts, and schedule
  them for auto-publish. Use when the user wants to create, manage, or publish LinkedIn
  content through LinWheel.
metadata:
  openclaw:
    emoji: "\U0001F3A1"
    requires:
      env: ["LINWHEEL_API_KEY"]
      tools: ["linwheel_analyze"]
---

# LinWheel Content Engine

> Turn any text into polished, scheduled LinkedIn posts via LinWheel tools.

## Execution Flow

```
Phase 1: Analyze — Assess content for LinkedIn potential, get suggested angles
Phase 2: Reshape — Decompose content into angle-specific draft posts (saved to LinWheel)
Phase 3: Refine — Run LLM editing passes on drafts (light/medium/heavy)
Phase 4: Visuals — Attach hero images and/or carousels to posts
Phase 5: Approve — Mark posts as approved for publishing
Phase 6: Schedule — Set publish times; LinWheel's cron auto-publishes
```

## When to Use

- Turning articles, buildlog entries, notes, or transcripts into LinkedIn posts
- Creating a batch of angle-specific posts from a single piece of source content
- Refining existing draft text with LinkedIn-optimized formatting
- Attaching typographic hero images or carousel slides to posts
- Managing the approval/scheduling pipeline for LinkedIn content
- Setting up or switching voice profiles for style matching

## Trigger Phrases

- "Turn this into a LinkedIn post"
- "Reshape this for LinkedIn"
- "Schedule my LinkedIn posts"
- "What LinkedIn drafts do I have?"
- "Create a LinkedIn carousel from this"
- "Pass this through linwheel"
- "Set up my LinkedIn voice"

---

## Authentication & Tool Access

Tools require `LINWHEEL_API_KEY` to be configured. No base URL needed — the SDK handles routing.

| Variable | Required | Description |
|----------|----------|-------------|
| `LINWHEEL_API_KEY` | Yes | API key starting with `lw_sk_...` |

### Tool Name Resolution

LinWheel tools are available under different prefixes depending on the agent runtime:

| Runtime | Tool Prefix | Example |
|---------|-------------|---------|
| **OpenClaw** (native extension) | `linwheel_` | `linwheel_analyze` |
| **Claude Code / MCP** | `mcp__linwheel__linwheel_` | `mcp__linwheel__linwheel_analyze` |

This skill uses the short `linwheel_*` names throughout. If you are in an MCP context, prepend `mcp__linwheel__` to each tool name. The parameters are identical across both runtimes.

---

## Tool Reference

### 1. Analyze Content

Assess text for LinkedIn posting potential. Always run this FIRST on new content.

**Tool:** `linwheel_analyze`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | Yes | The text content to analyze |
| `context` | string | No | Context about the content (e.g. "buildlog entry about shipping an MCP server") |

Returns: topic relevance scores, suggested angles with hook ideas, LinkedIn fit score (1-10), extractable insights, recommended post count.

---

### 2. Reshape Content into Posts

The primary content generation tool. Decompose source content into angle-specific posts.

**Tool:** `linwheel_reshape`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | Yes | Source content to reshape |
| `angles` | string[] | Yes | Angles to generate (see table below) |
| `preEdit` | boolean | No | Light-edit input before reshaping (default: false) |
| `instructions` | string | No | Tone, audience, or style instructions |
| `saveDrafts` | boolean | No | **Set true for normal workflow.** Saves posts to LinWheel. |

**Angles:**

| Angle | Voice | Best For |
|-------|-------|----------|
| `contrarian` | Challenges conventional wisdom | Hot takes, counter-narratives |
| `field_note` | Observational, from-the-trenches | Build logs, shipping stories |
| `demystification` | Breaks down complexity | Technical explainers |
| `identity_validation` | "You're not alone" resonance | Shared struggles |
| `provocateur` | Deliberately provocative | Engagement (use sparingly) |
| `synthesizer` | Connects disparate ideas | Cross-domain insights |
| `curious_cat` | Asks questions, explores | Open-ended exploration |

---

### 3. Refine a Draft

LLM editing pass at configurable intensity.

**Tool:** `linwheel_refine`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | Yes | Text to refine |
| `intensity` | string | No | `"light"`, `"medium"` (default), `"heavy"` |
| `instructions` | string | No | Custom editing instructions |
| `postType` | string | No | Angle for tone guidance |
| `saveDraft` | boolean | No | Save as new draft (default: false) |

**Intensities:** `light` = grammar only. `medium` = grammar + LinkedIn formatting. `heavy` = full rewrite.

---

### 4. Split Long Content into Series

**Tool:** `linwheel_split`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `text` | string | Yes | Long content to split |
| `maxPosts` | number | No | Max posts (2-10, default: 5) |
| `instructions` | string | No | Instructions for splitting |
| `saveDrafts` | boolean | No | Save all as drafts (default: false) |

---

### 5. Create a Manual Draft

Use when you already have finished post text.

**Tool:** `linwheel_draft`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fullText` | string | Yes | Full post text (max 3000 chars) |
| `hook` | string | No | Opening hook line (auto-extracted from first line if omitted) |
| `postType` | string | No | Content angle (default: `"field_note"`) |
| `approved` | boolean | No | Pre-approve (default: false) |
| `autoPublish` | boolean | No | Auto-publish at schedule (default: true) |
| `scheduledAt` | string | No | ISO 8601 datetime |

---

### 6. Bundle (Post + Visuals)

Create post + image + carousel atomically. At least one of image or carousel must be provided.

**Tool:** `linwheel_bundle`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fullText` | string | Yes | Post text |
| `hook` | string | No | Opening hook |
| `postType` | string | No | Content angle |
| `approved` | boolean | No | Pre-approve (default: false) |
| `autoPublish` | boolean | No | Auto-publish when scheduled (default: true) |
| `scheduledAt` | string | No | ISO 8601 scheduled time |
| `imageHeadlineText` | string | No | Hero image headline (max 200 chars). Omit to skip image. |
| `imageStylePreset` | string | No | Image style preset |
| `carouselSlides` | object[] | No | Carousel slides (1-10). Omit to skip carousel. |
| `carouselSlides[].headlineText` | string | Yes | Slide headline |
| `carouselSlides[].caption` | string | No | Slide caption |

---

### 7. List Posts

**Tool:** `linwheel_posts_list`

| Parameter | Type | Description |
|-----------|------|-------------|
| `approved` | `"true"/"false"` | Filter by approval |
| `scheduled` | `"true"/"false"` | Filter by scheduled |
| `published` | `"true"/"false"` | Filter by published |
| `type` | string | Filter by angle |
| `limit` | number | Max results (default: 50, max: 100) |
| `offset` | number | Pagination offset |

---

### 8. Get a Single Post

**Tool:** `linwheel_post_get`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID to retrieve |

---

### 9. Update a Post

Cannot edit posts already published to LinkedIn.

**Tool:** `linwheel_post_update`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID |
| `fullText` | string | No | New post text |
| `hook` | string | No | New hook line |
| `autoPublish` | boolean | No | Toggle auto-publish |

---

### 10. Approve / Unapprove

**Tool:** `linwheel_post_approve`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID |
| `approved` | boolean | Yes | `true` to approve, `false` to unapprove |

---

### 11. Schedule / Unschedule

**Tool:** `linwheel_post_schedule`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID |
| `scheduledAt` | string/null | Yes | ISO 8601 datetime, or `null` to clear |
| `autoPublish` | boolean | No | Toggle auto-publish (default: keeps current) |

A post auto-publishes when: `approved=true` AND `autoPublish=true` AND `scheduledAt` has arrived.

---

### 12. Generate Hero Image

**Tool:** `linwheel_post_image`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID |
| `headlineText` | string | Yes | Text on image (max 200 chars) |
| `stylePreset` | string | No | Default: `"typographic_minimal"` |

**Style presets:** `typographic_minimal`, `gradient_text`, `dark_mode`, `accent_bar`, `abstract_shapes`

---

### 13. Create Carousel

**Tool:** `linwheel_post_carousel`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `postId` | string | Yes | The post ID |
| `slides` | object[] | Yes | 1-10 slides |
| `slides[].headlineText` | string | Yes | Slide headline |
| `slides[].caption` | string | No | Slide caption |
| `stylePreset` | string | No | Visual style |

First slide = title. Last slide = CTA. Middle = content.

---

## Voice Profiles

Voice profiles inject writing style into all content generation. The active profile's samples are used for style matching in reshape, refine, split, and bundle operations.

### List Voice Profiles

**Tool:** `linwheel_voice_profiles_list`

No parameters. Returns all profiles with active status.

### Create Voice Profile

**Tool:** `linwheel_voice_profile_create`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Profile name |
| `samples` | string[] | Yes | Writing samples (3+ recommended) |
| `description` | string | No | Style notes (e.g. "Technical, direct, slightly irreverent") |
| `isActive` | boolean | No | Set as active profile (default: true) |

### Delete Voice Profile

**Tool:** `linwheel_voice_profile_delete`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `profileId` | string | Yes | The profile ID to delete |

### Activate Voice Profile

**Tool:** `linwheel_voice_profile_activate`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `profileId` | string | Yes | The profile ID to activate |

Only one profile can be active at a time. Activating one deactivates the previous.

---

## End-to-End Workflows

### Workflow A: Article to Multiple Posts (Standard)

```
1. ANALYZE    — linwheel_analyze (get angles + fit score)
2. RESHAPE    — linwheel_reshape (saveDrafts=true, use suggested angles)
3. LIST       — linwheel_posts_list (approved="false", review drafts)
4. REFINE     — linwheel_refine (polish individual posts, intensity="medium")
5. UPDATE     — linwheel_post_update (apply refined text to each post)
6. IMAGE      — linwheel_post_image (attach hero images)
7. APPROVE    — linwheel_post_approve (approved=true)
8. SCHEDULE   — linwheel_post_schedule (stagger across days)
```

### Workflow B: Quick Single Post

```
1. BUNDLE     — linwheel_bundle (post + image, approved=false)
2. REVIEW     — linwheel_post_get (check the draft)
3. APPROVE    — linwheel_post_approve (approved=true)
4. SCHEDULE   — linwheel_post_schedule
```

### Workflow C: Long Content to Series

```
1. SPLIT      — linwheel_split (saveDrafts=true)
2. REFINE     — linwheel_refine (each post)
3. UPDATE     — linwheel_post_update (apply refined text)
4. CAROUSEL   — linwheel_post_carousel (optional, first post)
5. APPROVE    — linwheel_post_approve (all posts)
6. SCHEDULE   — linwheel_post_schedule (consecutive business days, same time)
```

### Workflow D: Voice Setup

```
1. LIST       — linwheel_voice_profiles_list (check existing profiles)
2. CREATE     — linwheel_voice_profile_create (3+ writing samples)
3. VERIFY     — linwheel_voice_profiles_list (confirm active)
```

---

## Scheduling Best Practices

- LinkedIn peak: weekdays 8-10 AM and 12-1 PM in target timezone
- Minimum 4 hours between posts. Ideal: 1-2 per day max.
- Series: consecutive business days, same time each day
- Always create with `approved: false` first. Review, then approve separately.

## Quality Checklist

- [ ] Analyzed before reshaping (never skip analyze)
- [ ] Angles chosen from analyze results, not guessed
- [ ] All posts saved as drafts (`saveDrafts: true`)
- [ ] Posts created with `approved: false`
- [ ] Voice profile active if user has one
- [ ] Schedule times during LinkedIn peak hours
- [ ] No two posts from same source on same day
- [ ] User shown summary of all created/scheduled posts
