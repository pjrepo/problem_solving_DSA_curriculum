# The Weekly Rhythm

Every week of this course runs on the same machine. Once students internalise the rhythm (by about week 3), the structure disappears and they just work.

---

## 1. The week at a glance

| Day | What happens | Student hours |
|---|---|---|
| **Tuesday** | Weekend set **due at session start** · 15-min debrief · new topic · 2h assignment | 2.5 + 2 |
| **Wednesday** | New topic · 2h assignment | 2.5 + 2 |
| **Thursday** | New topic · 2h assignment | 2.5 + 2 |
| **Friday** | **The Arena** — contest / mock / checkpoint · 2h assignment (upsolve + editorial) | 2.5 + 2 |
| **Sat–Mon** | 6h weekend set, submitted Tuesday morning | 6 |
| | **Weekly total** | **~24 h** |

**Three new topics per week, not four.** Friday buys consolidation. This is the most important structural decision in the course — see the charter, §3, Pillar 2.

---

## 2. Teaching session template (Tue / Wed / Thu)

2 hours core, 30 minutes flex. If your session is 2h, drop the flex block; if 2.5h, run it.

| Time | Block | Notes |
|---|---|---|
| **0:00–0:10** | **Retrieval warm-up** | 3 questions from earlier weeks, cold, no notes. This is the retention engine — do not skip it when running late. On Tuesday this slot becomes the weekend-set debrief (15 min). |
| **0:10–0:40** | **Concept derivation** | Board/projector. Derive *why the pattern exists* — start from the brute force, find where it wastes work, invent the improvement. Never present a technique as a finished artefact. |
| **0:40–1:00** | **Live-code the template** | You type it, from scratch, in Python. Narrate every decision. Call out the JavaScript delta where it matters. Students type along. |
| **1:00–1:10** | Break | |
| **1:10–1:50** | **Guided practice** | Students solve 2–3 problems in the lab. You circulate — with 14 students you can reach everyone twice. |
| **1:50–2:00** | **Live code critique** | One student's solution on the projector, reviewed by the room. Rotate so everyone is critiqued ~4× per term. |
| **2:00–2:30** | **Flex block** | Harder variant, upsolve, or an awareness-level topic. Cut first when behind. |

Detailed protocol for each block: `01-instructor/01-session-playbook.md`.

---

## 3. The Arena (Friday)

Friday never carries new content. It runs in one of three modes:

### Contest mode (weeks 2, 3, 5, 7, 8, 10, 12, 13, 14 — nine contests)
- **0:00–0:15** — setup, rules, machines ready
- **0:15–1:45** — **90-minute timed contest**, 4 problems, ascending difficulty, live leaderboard
- **1:45–2:00** — immediate reveal: who solved what, the intended approach for each in 3 minutes
- **2:00–2:30** — *flex:* full editorial for the problem fewest students solved

### Mock interview mode (weeks 4, 9, 15 — three rounds)
- 7 pairs, **two rounds of 45 minutes** so every student both interviews and is interviewed
- You rotate between pairs, scoring against the mock rubric
- **15-minute group debrief** on the failure patterns you observed
- Week 15 is instructor-conducted in full FAANG format, not peer

### Checkpoint mode (weeks 6, 11, 16 — three graded assessments)
- Formal assessment covering everything to date, graded against a published rubric

Full protocols: `01-instructor/02-arena-playbook.md`.

---

## 4. Assignments

### Daily assignment (2h × 4 per week)

Set at the end of each session, on that day's topic, while it is still warm. **3–4 problems**, each with a stated time budget so students can self-diagnose when they're stuck too long.

**Friday's daily assignment is always the same shape:** upsolve every contest problem you did not finish, plus write one editorial. This converts the contest from a scoreboard into a learning event, and it is why the contests are worth 2 hours of class time.

### Weekend set (6h, due Tuesday at session start)

| Component | Time | Content |
|---|---|---|
| **New problems** | 4h | 5–6 problems on the week's topics, spanning the difficulty band |
| **Spaced revision** | 1h | 2 problems from **weeks N−1, N−3 and N−6** — see `01-instructor/04-spaced-revision-system.md` |
| **Written editorial** | 1h | One problem, written up properly (below) |

### What a written editorial must contain

Roughly 400–600 words. Not a solution dump — a *reasoning* document:

1. **Restate the problem** in your own words, including the constraints
2. **The brute force**, and its complexity
3. **The observation** that unlocks the better solution — the actual insight
4. **The approach**, in prose before any code
5. **Why it is correct** — an argument, not an assertion
6. **Time and space complexity**, justified
7. **What you would say in the first 60 seconds of an interview** if handed this problem

Item 7 is the one students want to skip and the one that matters most.

---

## 5. Submission mechanics

Each student keeps one repository, structured:

```
dsa-<name>/
├── week-01/
│   ├── tue/           # one file per problem, named <lc-number>-<slug>.py
│   ├── wed/
│   ├── thu/
│   ├── fri/
│   └── weekend/
│       ├── problems/
│       └── editorial.md
├── week-02/
└── progress-tracker.md
```

**Rules:**
- Every solution file starts with a comment block: problem link, approach in one line, time and space complexity
- Commit as you go — one commit per problem, not one commit per week
- LeetCode "Accepted" is the correctness bar; your review is the *quality* bar
- Late weekend sets are accepted until Wednesday at 50% — after that they become revision material, not credit

---

## 6. The instructor's week

| When | Task | Time |
|---|---|---|
| Tue before class | Skim 14 weekend sets, pick 2 problems to debrief and 1 solution to critique | 45 min |
| Tue/Wed/Thu after class | Spot-check daily submissions for red flags (copied solutions, no progress) | 20 min/day |
| Fri after Arena | Record contest results in the tracker | 15 min |
| Weekend | **Read all 14 editorials properly.** This is your highest-value hour of the week | 60–90 min |
| Weekend | Update the red list (failed problems to requeue) and flag at-risk students | 20 min |

**~4 hours per week outside class.** That is what makes a 14-student cohort worth having — this workload is impossible at 60 students, and it is the mechanism by which nobody quietly falls behind.
