# Curriculum Charter

*Problem Solving & DSA — Leapstart School of Technology*
*16 weeks · 14 students · 2nd year Computer Science*

---

## 1. The promise

At the end of week 16, a student who has done the work can walk into a Big Tech DSA round and:

| Capability | Operational definition |
|---|---|
| **Solve a fresh Medium** | Reads an unseen LeetCode Medium, identifies the pattern within 3 minutes, and produces working, readable code in **20–25 minutes total** |
| **State complexity correctly** | Gives time *and* space complexity unprompted, and can justify it — not recite it |
| **Think out loud** | Narrates the approach *before* coding, asks clarifying questions, and does not go silent under pressure |
| **Test their own code** | Walks through their solution on an example and an edge case without being asked |
| **Attempt a Hard** | On core patterns (DP, graphs, intervals, trees), makes real progress on a Hard even if unfinished — which is often enough to pass |

That is the bar. Everything in this curriculum is reverse-engineered from it.

---

## 2. The starting point (be honest about this)

Students arrive with **functions, loops and conditionals**. That is all we assume.

They do **not** have:

- Hash maps, sets, or any collection beyond a basic list
- Classes or objects
- Recursion
- Any notion of Big-O or algorithmic cost

This gap is the central design problem of the course. A conventional DSA syllabus that opens with "arrays and two pointers" assumes a hash map fluency these students do not have, and it loses half the cohort by week 3. So:

- **Week 1 teaches no algorithms.** It builds the language toolkit and the vocabulary of cost.
- **OOP is taught inline in Week 5**, at the exact moment a linked-list `Node` requires it.
- **Recursion gets an entire week (Week 7)**, not a lecture. It is the single largest failure point for loop-only programmers, and it gates trees, backtracking, divide-and-conquer and all of dynamic programming.

## 3. The four pillars

**Pillar 1 — Patterns over problems.**
Nobody memorises 300 solutions. Students learn ~20 patterns, each with a trigger signal ("sorted array + find a pair" → two pointers) and a canonical template. Problems are vehicles for patterns, never the point in themselves. See `03-reference/02-pattern-catalogue.md`.

**Pillar 2 — Retention is engineered, not hoped for.**
Week 2 material is gone by week 10 unless you fight for it. Every session opens with a retrieval warm-up. Every weekend set spends an hour on problems from weeks N−1, N−3 and N−6. This is the difference between a course students *took* and a skill they *have*.

**Pillar 3 — Articulation is a graded skill.**
Most students who fail DSA rounds *can* solve the problem. They fail because they code in silence, never clarify the input, and cannot explain why their solution is correct. So: every weekend set includes a written editorial, every Friday Arena is public performance, and there are three full mock interview rounds.

**Pillar 4 — Fourteen students is a superpower.**
This cohort size makes things possible that a class of 120 cannot do: you can read all 14 editorials, you can reach every student during guided practice, you can run real observed mock interviews, and you can spot a struggling student in week 3 instead of week 12. The design leans on this deliberately.

---

## 4. Explicit non-goals

We are optimising for interview performance in 16 weeks. That means deliberately **not** teaching:

| Cut | Why |
|---|---|
| Segment trees, Fenwick/BIT | Cost 2+ weeks; essentially never asked in product-company interviews |
| Minimum spanning trees (Kruskal/Prim) | Classic syllabus filler; vanishingly rare in interviews |
| Network flow, bipartite matching | Competitive-programming territory, not interview territory |
| Suffix arrays / suffix automata | Same |
| Red-black / AVL rotation mechanics | Asked as trivia at most; the *properties* of a balanced BST are taught, the rotations are not |
| Sorting algorithm internals beyond merge/quick | Students need sorting as a **tool**, not as a museum of algorithms |
| Formal complexity proofs, Master theorem drills | We build cost intuition instead; rigour here does not convert to interview performance |

Two topics are taught at **awareness level only** — recognised and explained, never implemented from scratch: **KMP** and **Rabin–Karp**. If a student is asked about string matching, they can hold a conversation; we do not spend a session on the failure function.

We also do not cover system design, low-level design, or behavioural rounds. This is a DSA course.

---

## 5. What the 370 hours buy

| Channel | Hours |
|---|---|
| Teaching sessions (Tue/Wed/Thu × 16) | ~120 h |
| Friday Arena (contests, mocks, checkpoints) | ~40 h |
| Daily assignments (4 × 2h × 16) | 128 h |
| Weekend sets (6h × 16) | 96 h |
| **Total student commitment** | **~384 h** |

Assigned problem volume lands at roughly **320 problems**, plus ~120 walked through in class. For calibration: Blind 75 is 75 problems and NeetCode 150 is 150. A serious self-directed candidate solves 300–500 before interviewing. This course delivers that volume *with* instruction, feedback and pressure-testing attached.

---

## 6. Honest risks

**This is a heavy load.** ~23 hours/week on one subject, for second-years carrying other courses. Watch for burnout around weeks 6–8 (post-checkpoint slump) and weeks 12–14 (DP fatigue). `01-instructor/05-intervention-guide.md` covers what to do. Every week file ends with a **"what to cut if you're behind"** section — use it without guilt.

**The recursion wall is real.** If a meaningful fraction of the cohort has not internalised recursion by end of Week 7, do not proceed to Week 8 as scheduled. Trees, backtracking and DP all collapse without it. Spending Week 8's flex blocks on remedial recursion is a far better trade than pushing a broken foundation forward.

**Volume can become theatre.** A student who "completes" 20 problems a week by reading solutions after 10 minutes learns nothing. The written editorial and the live code critique exist specifically to detect this. Enforce them.

---

## 7. Related documents

- `02-weekly-rhythm.md` — how a week actually runs
- `03-topic-map.md` — the full 16-week sequence and dependency chain
- `01-instructor/01-session-playbook.md` — how to run a lab session
- `01-instructor/03-assessment-and-rubrics.md` — grading scheme and all rubrics
