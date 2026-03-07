# 7e Quality Scoring Rubric

## Overview

LLM-as-judge evaluation for pipeline output quality. Each dimension scored 1-10. Pass criteria: overall mean ≥ 7.0, no single dimension < 5.0.

## Dimensions

### 1. Evidence Grounding (weight: 3x)

Does the output cite real, verifiable evidence?

| Score | Criteria |
|-------|----------|
| 9-10 | Every claim has a URL or specific citation. Stats include source + date. |
| 7-8 | Most claims evidenced. 1-2 general assertions without specific sources. |
| 5-6 | Mix of evidenced and unsupported claims. Some stats without sources. |
| 3-4 | Mostly assertions. Few specific citations. |
| 1-2 | No evidence. Pure opinion presented as fact. |

### 2. Scoring Discipline (weight: 2x)

Are quantitative scores consistent and justified?

| Score | Criteria |
|-------|----------|
| 9-10 | SDP weights applied correctly. Scores match evidence strength. Kill criteria have hard thresholds. |
| 7-8 | Scores mostly justified. Minor inconsistencies between evidence and ratings. |
| 5-6 | Some scores feel arbitrary. Weights may not be applied consistently. |
| 3-4 | Scores disconnected from evidence. Optimistic bias evident. |
| 1-2 | Numbers invented. No relationship between evidence and scores. |

### 3. Prose Quality (weight: 2x)

Is the writing clear, precise, and appropriate for the audience?

| Score | Criteria |
|-------|----------|
| 9-10 | Sharp, specific language. No cliches. Correct register for target audience. Rhythmic variation. |
| 7-8 | Clear and professional. Occasional generic phrasing. |
| 5-6 | Readable but bland. Corporate-speak or AI-sounding patterns. |
| 3-4 | Confusing structure. Jargon without explanation. |
| 1-2 | Incoherent or clearly unedited AI slop. |

### 4. Actionability (weight: 2x)

Can someone act on this output without additional research?

| Score | Criteria |
|-------|----------|
| 9-10 | Clear next steps with deadlines. Kill criteria with thresholds. Specific tactical recommendations. |
| 7-8 | Actionable recommendations but some gaps in specificity. |
| 5-6 | General advice. "Consider doing X" without how or when. |
| 3-4 | Vague conclusions. No clear path forward. |
| 1-2 | Pure analysis with no recommendations. |

### 5. Cross-Reference Integrity (weight: 1x)

Are upstream artifacts correctly referenced and threaded?

| Score | Criteria |
|-------|----------|
| 9-10 | All input_refs present and valid. Wikilinks resolve. Frontmatter refs correct. |
| 7-8 | Most references correct. 1-2 missing or broken links. |
| 5-6 | References present but some are wrong or orphaned. |
| 3-4 | Minimal cross-referencing. Pipeline threading broken. |
| 1-2 | No references to upstream artifacts. |

## Scoring Formula

```
weighted_score = (
  evidence * 3 +
  scoring_discipline * 2 +
  prose_quality * 2 +
  actionability * 2 +
  cross_ref_integrity * 1
) / 10

pass = weighted_score >= 7.0 AND min(all_dimensions) >= 5.0
```

## Judge Prompt Template

```
You are evaluating the output of a product-discovery pipeline skill.

Score the following output on these 5 dimensions (1-10 each):
1. Evidence Grounding — are claims backed by specific, verifiable evidence?
2. Scoring Discipline — are quantitative scores justified and consistent?
3. Prose Quality — is the writing clear, precise, and audience-appropriate?
4. Actionability — can someone act on this without further research?
5. Cross-Reference Integrity — are upstream artifacts correctly referenced?

For each dimension, provide:
- Score (1-10)
- One sentence justification

Then provide:
- Weighted overall score (using weights 3x, 2x, 2x, 2x, 1x)
- Pass/fail determination (pass = overall >= 7.0 AND no dimension < 5.0)
- Top 2 improvement suggestions

Output format:
{
  "evidence_grounding": { "score": N, "justification": "..." },
  "scoring_discipline": { "score": N, "justification": "..." },
  "prose_quality": { "score": N, "justification": "..." },
  "actionability": { "score": N, "justification": "..." },
  "cross_ref_integrity": { "score": N, "justification": "..." },
  "weighted_overall": N.N,
  "pass": true/false,
  "improvements": ["...", "..."]
}
```
