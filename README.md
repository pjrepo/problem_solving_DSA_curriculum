# Problem Solving & DSA — 16-Week Curriculum

**Leapstart School of Technology** · 2nd-year Computer Science · 14 students
Zero DSA background → Big Tech interview ready

---

## What this is

A complete, executable 16-week curriculum: 64 session plans, ~320 named LeetCode problems with time budgets, 10 timed contests, 3 mock interview rounds, 3 graded checkpoints, and the rubrics and protocols to run all of it.

| | |
|---|---|
| **Duration** | 16 weeks × 4 sessions (Tue/Wed/Thu/Fri), 2h core + 30min flex |
| **Teaching sessions** | 48 (Tue/Wed/Thu) |
| **Arena sessions** | 16 (Friday — contests, mocks, checkpoints) |
| **Student load** | ~23 h/week · **368 h total** |
| **Problems** | 338 distinct, ~86 Easy / ~196 Medium / ~25 Hard |
| **Assumed prerequisites** | Functions, loops, conditionals. **Nothing else.** |
| **Languages** | Language-agnostic reasoning · Python primary · JavaScript fully supported |
| **Exit bar** | A fresh Medium in 20–25 min, clean code, complexity justified aloud |

---

## Start here

**Teaching it for the first time?** Read in this order:

1. **[`00-overview/01-curriculum-charter.md`](00-overview/01-curriculum-charter.md)** — what the course promises, what it deliberately does not teach, and the honest risks
2. **[`00-overview/02-weekly-rhythm.md`](00-overview/02-weekly-rhythm.md)** — how a week actually runs, minute by minute
3. **[`00-overview/03-topic-map.md`](00-overview/03-topic-map.md)** — the 16-week sequence and the dependency chain that makes the order non-negotiable
4. **[`01-instructor/01-session-playbook.md`](01-instructor/01-session-playbook.md)** — how to run a lab session
5. **[`02-weeks/week-01.md`](02-weeks/week-01.md)** — then just follow the week files

**Before Week 1**, give students [`00-overview/04-day-zero-setup.md`](00-overview/04-day-zero-setup.md) and [`04-student/01-student-handbook.md`](04-student/01-student-handbook.md).

---

## Directory map

```
00-overview/     What the course is, how a week runs, the topic map, day-zero setup
01-instructor/   Session playbook · Arena playbook · rubrics · spaced revision · interventions
02-weeks/        week-01.md … week-16.md — the core deliverable
03-reference/    Master problem index · pattern catalogue · code templates · Py/JS · complexity
04-student/      Student handbook · progress tracker template
05-optional-practice/  Ungraded, opt-in extra practice: 4 tracks per session, all 64 sessions
```

### Instructor files
| File | Use it for |
|---|---|
| [`01-session-playbook.md`](01-instructor/01-session-playbook.md) | Running the 2h lab: warm-up, derivation, live coding, critique |
| [`02-arena-playbook.md`](01-instructor/02-arena-playbook.md) | Contest protocol, full mock-interview protocol, checkpoints |
| [`03-assessment-and-rubrics.md`](01-instructor/03-assessment-and-rubrics.md) | Grading scheme, code-review / mock / editorial rubrics |
| [`04-spaced-revision-system.md`](01-instructor/04-spaced-revision-system.md) | The N−1 / N−3 / N−6 rule and the red list |
| [`05-intervention-guide.md`](01-instructor/05-intervention-guide.md) | Catching and recovering a struggling student early |

### Reference files
| File | Use it for |
|---|---|
| [`01-master-problem-index.md`](03-reference/01-master-problem-index.md) | Every problem by number, by pattern, and by contest |
| [`02-pattern-catalogue.md`](03-reference/02-pattern-catalogue.md) | 29 patterns: trigger signal → template → complexity |
| [`03-code-templates-python.md`](03-reference/03-code-templates-python.md) | The 21 canonical templates — teach exactly these forms |
| [`04-python-js-cheatsheet.md`](03-reference/04-python-js-cheatsheet.md) | Side-by-side, plus the 8 JavaScript traps |
| [`05-complexity-reference.md`](03-reference/05-complexity-reference.md) | Operation costs, the constraints→complexity shortcut |

### Optional practice
| File | Use it for |
|---|---|
| [`00-how-to-use-this.md`](05-optional-practice/00-how-to-use-this.md) | **Read this first.** Ungraded, opt-in extra practice for all 64 sessions — four tracks per session (Reinforce · Stretch · Revision · Interview follow-ups), 384 problems that appear nowhere in the assigned curriculum. **Never graded and never expected**, with an explicit gate telling students when not to open it: the instructor guide's warning that you should *not simply assign more volume* applies here more than anywhere else in the repo. |

---

## The shape of the 16 weeks

| Phase | Weeks | What happens |
|---|---|---|
| **I — Foundations** | 1–6 | Toolkit & complexity · arrays · two pointers & sliding window · strings & matrices · linked lists (+OOP) · stacks & queues |
| **II — Techniques** | 7–13 | **Recursion** · binary search & sorting · trees · heaps & tries · graphs · Union-Find, Dijkstra & greedy · **DP I** |
| **III — Integration** | 14–16 | **DP II** · backtracking, bits & design · simulation and consolidation |

**Friday is never new content.** Giving up 16 teaching sessions is what guarantees the contests, mocks and consolidation actually happen instead of being perpetually deferred.

---

## Six design decisions worth knowing before you teach it

**1. Week 1 teaches no algorithms.** Students arrive without hash maps, classes, recursion or Big-O. A course opening with "arrays and two pointers" loses half the cohort by week 3. Week 1 builds the language toolkit and the vocabulary of cost.

**2. Recursion gets a whole week (Week 7), not a lecture.** It is the biggest wall for loop-only programmers and it gates trees, divide-and-conquer, DP and backtracking. Week 6 teaches the call stack as a physical object specifically to prepare it. **Week 7 has an explicit go/no-go decision at the end** — see the week file.

**3. Retention is engineered.** Every session opens with a retrieval warm-up; every weekend spends an hour re-solving problems from weeks N−1, N−3 and N−6. Without this, week 2 material is gone by week 10.

**4. Articulation is graded.** 30% of the grade is for things other than correctness. Every weekend set includes a written editorial; there are three mock interview rounds. Most candidates who fail DSA rounds *can* solve the problem — they fail on communication.

**5. Scope is cut aggressively.** No segment trees, Fenwick trees, MST, network flow, suffix structures, or AVL rotations. KMP and Rabin–Karp are awareness-level only. Every hour goes to what is actually asked. Rationale for each cut is in the charter.

**6. Fourteen students is treated as an asset.** Reading all 14 editorials weekly, live code critique, reaching every student during guided practice, and personally-observed mock interviews are all built in as recurring structure. Instructor load outside class is ~4 h/week.

---

## Two things to verify before term starts

1. **Spot-check the LeetCode numbers.** Titles are the reliable identifier; numbers occasionally drift. The master index flags this.
2. **No Premium problems are assigned anywhere** — free-tier equivalents are used throughout, and the substitutions are listed in the master index.

---

## Adapting it

- **Fewer hours available?** Every week file ends with a **"what to cut if you are behind"** section, ordered, plus a list of what must never be cut.
- **A larger cohort?** The design assumes you can read 14 editorials a week. Above ~30 students, switch to sampled deep review and peer critique — see `01-instructor/03-assessment-and-rubrics.md`.
- **Weeks lost to holidays or exams?** Weeks 11 and 13 are already the lightest; Week 16 is consolidation and can absorb a compressed schedule.
- **Changed a week file?** Regenerate `03-reference/01-master-problem-index.md` from the week tables rather than hand-editing it.
