# Assessment & Rubrics

---

## 1. Weighting

| Component | Weight | What it measures |
|---|---|---|
| Daily assignments (64 sets) | **20%** | Consistency and volume |
| Weekend sets (16) | **25%** | Depth, retention, and written articulation |
| Arena contests (10) | **15%** | Performance under time pressure |
| Mock interviews (3) | **15%** | Communication and interview behaviour |
| Checkpoints (3) | **25%** | Cumulative mastery — 7% / 8% / 10% |

**Deliberately, 30% of the grade is for things other than getting the right answer.** Contests and mocks reward speed and communication, because those are what actually separate candidates who pass DSA rounds from candidates who can solve the problems at home.

### Daily assignment scoring (fast — this must take you under 20 min/day)

| Score | Criterion |
|---|---|
| **2** | All problems attempted, all with the header block and stated complexity |
| **1** | Partial completion, or headers/complexity missing |
| **0** | Not submitted |

Correctness is *not* scored here — LeetCode already gave that verdict. You are scoring the discipline. Spot-check 3–4 students' actual code per day on rotation; the live critique catches the rest.

---

## 2. Code Review Rubric

Used for checkpoints, weekend-set deep reviews, and live critique. **25 points.**

### Correctness — 8 points
| Pts | |
|---|---|
| 8 | Correct, and handles empty input, single element, duplicates, and boundary values |
| 6 | Correct on the main path; one edge case unhandled |
| 4 | Core logic sound; a real bug on non-trivial input |
| 2 | Approach is right; implementation does not work |
| 0 | Wrong approach |

### Complexity — 5 points
| Pts | |
|---|---|
| 5 | Optimal for the problem, stated correctly, **and justified** |
| 4 | Optimal, stated correctly, not justified |
| 3 | Sub-optimal but correctly analysed (honest and accurate beats optimal and wrong) |
| 1 | Stated incorrectly |
| 0 | Not stated |

### Readability — 6 points
| Pts | |
|---|---|
| 6 | A stranger reads it in 30 seconds. Names say what things hold. No dead code. |
| 4 | Readable with effort; some names uninformative (`a`, `temp`, `res2`) |
| 2 | Requires tracing to understand |
| 0 | Unreadable, or commented-out attempts left in |

### Idiom — 3 points
Uses the language properly: `collections.Counter` over a manual dict loop, `enumerate` over `range(len())`, a set for membership, `heapq` for a heap. Fighting the language costs points.

### Self-verification — 3 points
Evidence they tested it: a `__main__` block with cases, or a comment tracing an example. Silence here scores 0.

---

## 3. Mock Interview Rubric

**30 points.** Modelled on how Big Tech interviews are actually scored — note that only 10 of 30 points are for the code.

### Clarification & problem framing — 5 points
| Pts | |
|---|---|
| 5 | Restated the problem, asked about input size, types, duplicates, empty input, and expected output format before thinking about approach |
| 3 | Asked one or two clarifying questions |
| 1 | Asked nothing but did restate the problem |
| 0 | Started immediately |

### Approach & communication — 8 points
| Pts | |
|---|---|
| 8 | Stated a brute force with its cost, identified the waste, proposed the optimisation, **got agreement before coding** |
| 6 | Explained the intended approach clearly before coding |
| 4 | Explained partially; began coding mid-thought |
| 2 | Started coding, narrated afterwards |
| 0 | Silent |

### Coding — 10 points
Correct, complete, readable code, written while continuing to talk. Deduct for long silences even if the code is right — a silent candidate cannot be scored by a real interviewer either.

### Testing — 4 points
| Pts | |
|---|---|
| 4 | Walked through a normal case *and* an edge case unprompted, found and fixed any issue |
| 2 | Tested when prompted |
| 0 | Declared "done" without testing |

### Complexity analysis — 3 points
Stated time and space unprompted, correctly, with reasoning.

### Response to follow-up — 3 points
Handled "can you do better?" or an extension without visible panic. Engaged with the question rather than defending the existing solution.

### Passing bar
- **Week 4 (first mock):** 15/30 — this round is calibration, expect low scores and say so in advance
- **Week 9:** 20/30
- **Week 15:** **23/30 = would pass a real phone screen.** This is the number that matters.

---

## 4. Editorial Rubric

**10 points**, applied to the weekend written editorial. This is the fastest thing you grade and one of the most informative.

| Component | Pts | Looking for |
|---|---|---|
| Problem restated with constraints | 1 | In their own words, not copy-pasted |
| Brute force + its complexity | 1 | Present and correctly costed |
| **The key observation** | **3** | The actual insight, in their own words. If this is vague, they do not understand it — this is the single most diagnostic line in the whole document |
| Approach in prose before code | 1 | Readable without the code |
| Correctness argument | 2 | An argument, not "it works because it passes" |
| Complexity, justified | 1 | |
| **"First 60 seconds in an interview"** | **1** | What they would actually say aloud |

**Read all 14 of these every week.** It takes 60–90 minutes and it is the highest-value hour in your week: it is the only channel that shows you a student's *reasoning* rather than their output. A student who solves problems but writes vague observations is pattern-matching without understanding, and they will fail the first unfamiliar interview question they meet.

---

## 5. Contest scoring

Per contest: 4 points per problem solved, +1 for first solve, ranked by total then by time.

Contest grade over the term is **not** raw score — it is **60% participation and completion of upsolve, 40% performance trend against the student's own baseline**. This keeps the bottom of the cohort engaged, and it correctly rewards the student who goes from 1 problem to 3 over ten contests.

---

## 6. Tracking

One spreadsheet, 14 rows. Columns:

`Name · Language · Daily streak · Weekend sets (16) · Editorial avg /10 · Contest scores (10) · Contest trend · Mock 1 /30 · Mock 2 /30 · Mock 3 /30 · CP1 /25 · CP2 /25 · CP3 /25 · Red-list count · Flag`

**The `Flag` column is the important one.** Set it when any of these fire:

- Two consecutive weekend sets missed
- Editorial average below 5/10 for two weeks
- Bottom of the leaderboard for three consecutive contests
- Mock score below the round's bar
- Red list exceeding 8 unresolved problems

A flag means a conversation this week, not a note for later. With 14 students there is no excuse for a student to reach Week 12 in trouble without you having intervened at Week 5.
