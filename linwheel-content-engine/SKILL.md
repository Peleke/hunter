---
name: linwheel-content-engine
description: >
  Drive the LinWheel LinkedIn content engine via its REST Agent API. Analyze source
  content, reshape it into angle-specific LinkedIn posts, refine drafts with LLM editing,
  attach hero images and carousels, approve posts, and schedule them for auto-publish.
  Use when the user wants to create, manage, or publish LinkedIn content through LinWheel.
metadata:
  openclaw:
    emoji: "\U0001F3A1"
    requires:
      env: ["LINWHEEL_API_URL", "LINWHEEL_API_KEY"]
---

# LinWheel Content Engine

> Turn any text into polished, scheduled LinkedIn posts via LinWheel's REST Agent API.

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

## Trigger Phrases

- "Turn this into a LinkedIn post"
- "Reshape this for LinkedIn"
- "Schedule my LinkedIn posts"
- "What LinkedIn drafts do I have?"
- "Create a LinkedIn carousel from this"
- "Pass this through linwheel"

---

## Authentication

Every request MUST include:

```
Authorization: Bearer $LINWHEEL_API_KEY
Content-Type: application/json
```

| Variable | Required | Description |
|----------|----------|-------------|
| `LINWHEEL_API_URL` | Yes | Base URL (e.g., `https://linwheel.io`) |
| `LINWHEEL_API_KEY` | Yes | API key starting with `lw_sk_...` |
| `LINWHEEL_SIGNING_SECRET` | No | HMAC signing secret for request signatures |

### Request Template

```bash
curl -X {METHOD} "$LINWHEEL_API_URL{path}" \
  -H "Authorization: Bearer $LINWHEEL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{body_json}'
```

---

## API Reference

### 1. Analyze Content

Assess text for LinkedIn posting potential. Always run this FIRST on new content.

```
POST /api/agent/analyze
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `text` | string | Yes | The text content to analyze |
| `context` | string | No | Context about the content |

Returns: topic relevance, suggested angles, LinkedIn fit score, recommended post count.

---

### 2. Reshape Content into Posts

The primary content generation endpoint. Decompose source content into angle-specific posts.

```
POST /api/agent/reshape
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
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

```
POST /api/agent/refine
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `text` | string | Yes | Text to refine |
| `intensity` | string | No | `"light"`, `"medium"` (default), `"heavy"` |
| `instructions` | string | No | Custom editing instructions |
| `postType` | string | No | Angle for tone guidance |
| `saveDraft` | boolean | No | Save as new draft (default: false) |

**Intensities:** `light` = grammar only. `medium` = grammar + LinkedIn formatting. `heavy` = full rewrite.

---

### 4. Split Long Content into Series

```
POST /api/agent/split
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `text` | string | Yes | Long content to split |
| `maxPosts` | number | No | Max posts (2-10, default: 5) |
| `instructions` | string | No | Instructions for splitting |
| `saveDrafts` | boolean | No | Save all as drafts (default: false) |

---

### 5. Create a Manual Draft

Use when you already have finished post text.

```
POST /api/agent/draft
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `fullText` | string | Yes | Full post text (max 3000 chars) |
| `hook` | string | No | Opening hook line |
| `postType` | string | No | Content angle (default: `"field_note"`) |
| `approved` | boolean | No | Pre-approve (default: false) |
| `autoPublish` | boolean | No | Auto-publish at schedule (default: true) |
| `scheduledAt` | string | No | ISO 8601 datetime |

---

### 6. List Posts

```
GET /api/agent/posts
```

| Param | Type | Description |
|-------|------|-------------|
| `approved` | `"true"/"false"` | Filter by approval |
| `scheduled` | `"true"/"false"` | Filter by scheduled |
| `published` | `"true"/"false"` | Filter by published |
| `type` | string | Filter by angle |
| `limit` | number | Max results (default: 50, max: 100) |
| `offset` | number | Pagination offset |

---

### 7. Get a Single Post

```
GET /api/agent/posts/{postId}
```

---

### 8. Update a Post

```
PATCH /api/agent/posts/{postId}
```

| Field | Type | Description |
|-------|------|-------------|
| `fullText` | string | New post text |
| `hook` | string | New hook line |
| `autoPublish` | boolean | Toggle auto-publish |

---

### 9. Approve / Unapprove

```
POST /api/agent/posts/{postId}/approve
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `approved` | boolean | Yes | `true` to approve, `false` to unapprove |

---

### 10. Schedule / Unschedule

```
PATCH /api/agent/posts/{postId}/schedule
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `scheduledAt` | string/null | Yes | ISO 8601 datetime, or `null` to clear |
| `autoPublish` | boolean | No | Toggle auto-publish |

A post auto-publishes when: `approved=true` AND `autoPublish=true` AND `scheduledAt` has arrived.

---

### 11. Generate Hero Image

```
POST /api/agent/posts/{postId}/image
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `headlineText` | string | Yes | Text on image (max 200 chars) |
| `stylePreset` | string | No | Default: `"typographic_minimal"` |

**Style presets:** `typographic_minimal`, `gradient_text`, `dark_mode`, `accent_bar`, `abstract_shapes`

---

### 12. Create Carousel

```
POST /api/agent/posts/{postId}/carousel
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `slides` | object[] | Yes | 1-10 slides |
| `slides[].headlineText` | string | Yes | Slide headline |
| `slides[].caption` | string | No | Slide caption |
| `stylePreset` | string | No | Visual style |

First slide = title. Last slide = CTA. Middle = content.

---

### 13. Bundle (Post + Visuals)

Create post + image + carousel atomically.

```
POST /api/agent/bundle
```

| Field | Type | Description |
|-------|------|-------------|
| `fullText` | string | Post text (required) |
| `hook` | string | Opening hook |
| `postType` | string | Content angle |
| `approved` | boolean | Pre-approve (default: false) |
| `image.headlineText` | string | Hero image headline (omit to skip) |
| `image.stylePreset` | string | Image style |
| `carousel.slides` | object[] | Carousel slides (omit to skip) |

---

## End-to-End Workflows

### Workflow A: Article to Multiple Posts (Standard)

```
1. ANALYZE   — POST /api/agent/analyze (get angles + fit score)
2. RESHAPE   — POST /api/agent/reshape (saveDrafts=true, use suggested angles)
3. LIST      — GET  /api/agent/posts?approved=false (review drafts)
4. REFINE    — POST /api/agent/refine (polish individual posts, intensity=medium)
5. UPDATE    — PATCH /api/agent/posts/{id} (apply refined text)
6. IMAGE     — POST /api/agent/posts/{id}/image (attach hero images)
7. APPROVE   — POST /api/agent/posts/{id}/approve
8. SCHEDULE  — PATCH /api/agent/posts/{id}/schedule (stagger across days)
```

### Workflow B: Quick Single Post

```
1. BUNDLE    — POST /api/agent/bundle (post + image, approved=false)
2. REVIEW    — GET  /api/agent/posts/{id}
3. APPROVE   — POST /api/agent/posts/{id}/approve
4. SCHEDULE  — PATCH /api/agent/posts/{id}/schedule
```

### Workflow C: Long Content to Series

```
1. SPLIT     — POST /api/agent/split (saveDrafts=true)
2. REFINE    — POST /api/agent/refine (each post)
3. CAROUSEL  — POST /api/agent/posts/{id}/carousel (optional, first post)
4. APPROVE   — Approve all
5. SCHEDULE  — Consecutive business days, same time
```

---

## Scheduling Best Practices

- LinkedIn peak: weekdays 8-10 AM and 12-1 PM in target timezone
- Minimum 4 hours between posts. Ideal: 1-2 per day max.
- Series: consecutive business days, same time each day
- Always create with `approved: false` first. Review, then approve separately.

## Error Reference

| Status | Meaning | Action |
|--------|---------|--------|
| 401 | Bad API key or HMAC | Check `$LINWHEEL_API_KEY` |
| 404 | Post not found | Verify postId, list posts |
| 422 | Validation error | Check field constraints (3000 char limit, valid angles) |
| 429 | Rate limited | Wait and retry |

## Quality Checklist

- [ ] Analyzed before reshaping (never skip analyze)
- [ ] Angles chosen from analyze results, not guessed
- [ ] All posts saved as drafts (`saveDrafts: true`)
- [ ] Posts created with `approved: false`
- [ ] Schedule times during LinkedIn peak hours
- [ ] No two posts from same source on same day
- [ ] User shown summary of all created/scheduled posts
