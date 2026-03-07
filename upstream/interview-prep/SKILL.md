---
name: interview-prep
description: "Interview gap analysis and syllabus generator — takes a candidate's current state and a target role, identifies the gaps, and produces a phased study plan with resources, weekly schedule, success metrics, and a performance ramp (private camera → recorded reviews → live streaming → interviews feel easy)."
license: MIT
metadata:
  openclaw:
    emoji: "\U0001F9E0"
---

# Interview Prep — Gap Syllabus Generator

Takes where you are + where you want to be → produces a phased plan to close the gap.

Not a generic "study leetcode" guide. A syllabus calibrated to YOUR strengths, YOUR gaps, and the SPECIFIC interview format of your target role.

## When to Use

- You've identified a target role but aren't interview-ready
- You want a structured ramp from current state to interview-confident
- You need to prioritize: what to study first given limited time
- You want to build interview performance skills (not just knowledge)
- You're preparing for a specific company's interview format

## Trigger Phrases

- "How do I prep for [company] interviews?"
- "Build me a study plan for [role]"
- "What's the gap between me and [role]?"
- "Create an interview syllabus"
- "/interview-prep [company] [role]"
- "/interview-prep gap-analysis"

---

## Workflow: Gap Analysis

Identify the delta between current state and target role requirements.

### Inputs

1. **Candidate state** — what you can do RIGHT NOW (be honest)
   - Languages and proficiency level (production / comfortable / learning / none)
   - DSA level (can't solve easies / easy-comfortable / medium-comfortable / hard-capable)
   - System design level (never done it / can sketch / can deliver 45-min session / strong)
   - ML theory level (practitioner only / can explain basics / can whiteboard / can prove)
   - Domain knowledge relevant to role
   - Interview experience (none / some / confident)

2. **Target role** — from job-search brief or raw JD
   - Company and role title
   - Interview format (if known — research on Glassdoor, Blind, levels.fyi)
   - Technical requirements from JD
   - Seniority level expectations

3. **Timeline** — how long until you want to be ready
   - Urgent (2-4 weeks) — focus on what you CAN pass, skip what you can't
   - Standard (2-3 months) — close the most impactful gaps
   - Full ramp (6-9 months) — close everything, including hard problems and theory

4. **Available hours/week** — be realistic
   - 5 hrs/week (working full-time, minimal bandwidth)
   - 10 hrs/week (working but dedicated)
   - 20+ hrs/week (between jobs, full focus)

### Gap Matrix

For each interview dimension, score current state and target state:

```markdown
| Dimension | Current | Target | Gap | Priority | Close Time |
|-----------|---------|--------|-----|----------|-----------|
| DSA (easy) | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| DSA (medium) | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| DSA (hard) | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| System design | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| ML fundamentals | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| Domain knowledge | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| Language: {lang} | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| Behavioral/culture | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
| Performance (nerves) | {1-5} | {1-5} | {delta} | {H/M/L} | {weeks} |
```

### Priority Rules

- **If interview is portfolio/practical**: deprioritize DSA, focus on project articulation and system design
- **If interview is DSA-heavy**: this is the bottleneck, spend 60%+ of time here
- **If interview has ML theory**: prioritize fundamentals you already USE but can't whiteboard
- **If interview has system design**: practice with YOUR systems (you built them, now explain them under time pressure)
- **Performance (nerves) is ALWAYS high priority** — the best knowledge fails under anxiety

---

## Workflow: Syllabus

Generate a phased study plan from the gap analysis.

### Phase Structure

Every syllabus has 3 phases, regardless of timeline:

**Phase 1: Foundation** (first 30% of timeline)
- Close the highest-priority gaps to "functional" level
- Build daily habits (problem-solving, reading, practice)
- Start the performance ramp (camera)
- Target: "I won't embarrass myself"

**Phase 2: Depth** (middle 40% of timeline)
- Push from functional to strong on priority gaps
- Start harder problems / more complex systems
- Advance performance ramp (recorded reviews)
- Target: "I can handle most of what they throw at me"

**Phase 3: Polish** (final 30% of timeline)
- Company-specific prep (research their blog, their interview format, their culture)
- Mock interviews (with friends, Pramp, or self-recorded)
- Performance ramp peaks (live sessions if applicable)
- Target: "I'm ready. The interview is just a conversation."

### Syllabus Template

```markdown
# Interview Syllabus: {Company} — {Role}

## Gap Summary
{1-2 sentence honest assessment}

## Phase 1: Foundation ({start} — {end})

### DSA
- **Resource**: {specific resource}
- **Pace**: {problems/day}
- **Focus**: {topic order}
- **Method**: {how to practice — timer, review, re-solve}
- **Milestone**: {specific measurable target}

### {Other gap area}
- **Resource**: {specific resource}
- **Pace**: {amount/day}
- **Focus**: {topic order}
- **Method**: {how to practice}
- **Milestone**: {specific measurable target}

### Performance
- **Stage**: Private camera
- **Method**: {what to do}
- **Milestone**: {specific target}

## Phase 2: Depth ({start} — {end})
{same structure, harder targets}

## Phase 3: Polish ({start} — {end})
{company-specific prep}

## Weekly Schedule
| Day | Morning ({N} min) | Evening ({N} min) |
|-----|-------------------|-------------------|
| Mon | {activity} | {activity} |
| ... | ... | ... |
| Sun | Rest | Rest |

Total: {N} hours/week

## Success Metrics
| Phase End | DSA | {Gap 2} | {Gap 3} | Performance |
|-----------|-----|---------|---------|-------------|
| Phase 1 | {target} | {target} | {target} | {target} |
| Phase 2 | {target} | {target} | {target} | {target} |
| Phase 3 | {target} | {target} | {target} | {target} |

## Resources
- **DSA**: {specific books, sites, courses}
- **{Gap 2}**: {resources}
- **System Design**: {resources}
- **Mock Interviews**: {platforms}
- **Company-Specific**: {blog posts, docs, papers to read}
```

---

## Workflow: Performance Ramp

Interview anxiety is the silent killer. You can know everything and freeze under observation.
The fix: **progressive exposure**. Increase the observation pressure until the interview is the easy version.

### The 4-Stage Ramp

#### Stage 1: Private Camera
- Solve problems while recording yourself (phone, screen share, whatever)
- Talk out loud. Explain your thinking to the camera.
- Watch it back. Notice: where you go silent, where you panic, where you ramble, where you lose the thread
- **Weekly test**: pick a random problem at your edge level, set a timer, film it start to finish
- **Target**: solve a problem on camera without freezing or going silent for >10 seconds

#### Stage 2: Recorded Reviews
- Record yourself doing harder problems. Post to a small audience (friend, Discord, unlisted YouTube)
- Get feedback: "you lost me at minute 3" or "your approach was clean but you didn't explain WHY"
- **Weekly test**: 1 problem at the next difficulty tier, filmed, shared
- **Target**: clean narration through a problem, even when you're stuck

#### Stage 3: Live Sessions
- Stream study sessions live (Twitch, YouTube Live, Discord stage)
- Chat can watch you think. This is maximum observation pressure.
- Start with easier problems (confidence), gradually increase difficulty
- **Weekly**: 1-2 live sessions, mix of difficulties
- **Target**: confident and conversational while solving, even when stuck

#### Stage 4: Mock Interviews
- Full mock with another person (Pramp, friend, paid service)
- Simulate the exact format: 45-minute timer, problem delivered cold, whiteboard/shared doc
- Debrief after: what went well, what didn't, specific improvements
- **Weekly**: 1 mock per week in the final phase
- **Target**: feel bored in the mock because you've done harder (live streaming)

### Why This Works

```
Private camera → Recorded review → Live stream → Mock interview → Real interview
     (easy)         (moderate)        (hard)        (harder)         (easiest)
```

Each stage increases observation pressure. By the time you reach the real interview, it's the lowest-pressure performance you've done in months.

### The Twitch Strategy (Optional but Powerful)

If you stream interview prep publicly:
- You build an audience of people in the same boat (community)
- You become "the person who preps in public" (reputation)
- You can say in interviews: "I stream this live on Twitch" (confidence signal)
- Chat roasting you is harder than a polite interviewer (exposure therapy)
- Clips of you solving problems become portfolio pieces

---

## Workflow: Company Research

Before generating a company-specific syllabus, research their interview format.

### Sources
- Glassdoor interview reviews (search "{company} software engineer interview")
- Blind (TeamBlind) threads
- levels.fyi interview section
- Company engineering blog (indicates what they value technically)
- Reddit r/cscareerquestions (search for company name)
- Direct: ask on LinkedIn "anyone interviewed at {company} recently?"

### What to Capture
- Number of rounds and format of each
- Coding: leetcode style? Take-home? Pair programming? Live coding?
- System design: present? What level of depth?
- ML/domain: how deep? Papers? Fundamentals only?
- Behavioral: structured (STAR)? Conversational? Values-based?
- Timeline: how long from application to offer?
- Known red flags or tips from past candidates

### Output
Add to the company's application brief under a `## Interview Format` section.

---

## Integration with Job Search Skill

This skill consumes output from `job-search`:
- Application briefs provide the target role requirements
- Gap analysis from briefs becomes input to the syllabus
- The sort workflow's "interview readiness" dimension feeds from this skill's assessment

This skill produces:
- Syllabi saved to `job-search/assets/{company}-ramp.md` or a standalone location
- Performance ramp plans
- Updated readiness assessments (feed back into job-search sort)

---

## Quality Checklist

- [ ] Gap analysis is brutally honest — don't inflate current state
- [ ] Syllabus has specific resources, not "study algorithms"
- [ ] Every phase has measurable milestones, not "get better at X"
- [ ] Weekly schedule is realistic for the candidate's available hours
- [ ] Performance ramp is included — knowledge without performance fails
- [ ] Company research informs the emphasis (don't prep DSA for a portfolio-based interview)
- [ ] Timeline accounts for the candidate's urgency (income needed NOW vs. dream role later)
- [ ] Rest days are in the schedule — burnout kills consistency
