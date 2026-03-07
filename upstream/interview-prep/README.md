# Interview Prep — Gap Syllabus Generator

Takes where you are + where you want to be → produces a phased study plan to close the gap.

## What It Does

1. **Gap Analysis** — honest assessment of current state vs. target role requirements
2. **Syllabus Generation** — phased study plan with resources, schedule, milestones
3. **Performance Ramp** — progressive exposure (camera → recordings → live stream → mocks → interviews)
4. **Company Research** — interview format intel from Glassdoor, Blind, levels.fyi

## How It Works

```
Candidate state + Target role → Gap matrix → Phased syllabus → Weekly schedule
                                                    ↓
                                          Performance ramp (camera → Twitch → interviews)
```

## Integration

- Consumes: `job-search` application briefs (target role requirements, gap analysis)
- Produces: syllabi, readiness assessments (feed back into job-search sort)

## Example

```
/interview-prep anthropic ai-observability
```

→ Produces a 6-month syllabus covering DSA (easy → hard), ML fundamentals, system design (using qortex as base system), and a performance ramp from private camera recordings to live Twitch streams.
