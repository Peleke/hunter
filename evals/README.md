# Hunter Pipeline Eval Framework (7b-e)

Four-level evaluation system for pipeline quality assurance.

## Levels

```
7b: Input Validation    → Does it reject bad envelopes?     (Haiku, every PR)
7c: Output Conformance  → Does output match the schema?     (Haiku, every PR)
7d: Pipeline Integration → Does the full chain connect?     (Sonnet, release)
7e: Quality Scoring     → Is the output actually good?      (Opus, release)
```

## Directory Structure

```
evals/
├── 7b-input/
│   └── fixtures/
│       └── invalid-envelopes.json    # 20 invalid envelope fixtures
├── 7c-output/
│   ├── fixtures/
│   │   └── valid-envelopes.json      # 10 valid envelope fixtures
│   └── schemas/                      # Per-skill output schemas (symlinked)
├── 7d-pipeline/                      # Full-chain integration tests
├── 7e-quality/
│   ├── rubrics/
│   │   └── pipeline-quality.md       # 5-dimension scoring rubric
│   └── calibration/                  # Human-reviewed baseline scores
└── README.md                         # This file
```

## Running Evals

### 7b + 7c (PR gate)

Validate envelope structure against `schemas/pipeline-envelope.schema.json`:

```bash
# Validate all invalid fixtures are rejected
node evals/run-7b.js

# Validate all valid fixtures pass
node evals/run-7c.js
```

### 7d (Integration)

Run the full pipeline chain on a test domain:

```bash
# Requires ANTHROPIC_API_KEY (Sonnet)
node evals/run-7d.js --domain "test-domain"
```

### 7e (Quality)

Score pipeline outputs against the quality rubric:

```bash
# Requires ANTHROPIC_API_KEY (Opus)
node evals/run-7e.js --input evals/7d-pipeline/latest-run.json
```

## Pass Criteria

| Level | Pass | Fail |
|-------|------|------|
| 7b | 100% of invalid envelopes rejected | Any invalid envelope accepted |
| 7c | 100% of valid envelopes pass schema | Any valid envelope rejected |
| 7d | Full chain completes, all refs connected | Chain breaks or refs orphaned |
| 7e | Overall ≥ 7.0, no dimension < 5.0 | Below threshold on any metric |

## Cost Estimates

| Level | Model | Per Run | Frequency |
|-------|-------|---------|-----------|
| 7b | Schema validation (no LLM) | $0 | Every PR |
| 7c | Schema validation (no LLM) | $0 | Every PR |
| 7d | Sonnet | ~$5-15 | Release |
| 7e | Opus | ~$10-20 | Release |
