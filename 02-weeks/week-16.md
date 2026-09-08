# Week 16 — Simulation & Consolidation

**Phase III: Integration** · **No new content** · Friday: **Final Assessment (10%)**

> The whole course points here. Fifteen weeks taught roughly 29 patterns and 320 problems; this week converts that into the thing interviews actually measure — **looking at an unfamiliar problem and knowing, within three minutes, what kind of problem it is.**
>
> Nothing new is taught. Tuesday drills recognition in isolation. Wednesday puts it under interview conditions. Thursday fixes what your data says is broken and hands them a plan for after the course. Friday measures it.

---

## Exit criteria

- [ ] Identify the pattern of an unseen problem within 3 minutes, and justify it from the statement
- [ ] Distinguish confusable patterns and say **why it is not** the one it resembles
- [ ] Solve two Mediums in 45 minutes while narrating, in interview conditions
- [ ] State time and space complexity unprompted, with justification, every time
- [ ] Leave with a written, dated plan for the 90 days after this course

---

## Session 1 (Tuesday) — Pattern Recognition Drill

> **No coding today.** Recognition is a separate skill from implementation, and it is the one that is never practised in isolation.

### Weekend-set debrief (0:00–0:15)
Debrief **LC 146 LRU Cache** — look for "the map stores node references." Then the duplicate-handling cluster (LC 90 and LC 40): confirm the per-level skip rule landed.

### Framing (0:15–0:25)

> *"In a real interview you have about three minutes between hearing the problem and needing to say something intelligent. That window is not about coding — it is about classification. Today we practise only that."*

**The method — five questions, in order:**
1. **What is the input?** Array, string, tree, graph, grid, stream? Sorted?
2. **What is the output?** A count, a maximum, a boolean, a list, a structure?
3. **What do the constraints say?** n ≤ 20 means exponential is fine; n ≤ 10⁵ means O(n log n) at worst.
4. **Which phrase is the trigger?** "contiguous", "prefix", "shortest path", "how many ways", "in place", "k largest", "prerequisites".
5. **What is the brute force, and what does it waste?** The waste names the fix.

### The drill (0:25–1:50)

**15 unseen problems.** For each, students get **3 minutes** to write down, on paper:
- The pattern
- The trigger phrase that identified it
- A one-line approach
- The expected time and space complexity

Then **3 minutes** of discussion. **No code is written all session.**

| # | Problem | Pattern | Trigger to look for |
|---|---|---|---|
| 1 | LC 1071 Greatest Common Divisor of Strings | Math / string | repeated structure, divisibility |
| 2 | LC 2352 Equal Row and Column Pairs | Hash map counting | "count matching pairs" |
| 3 | LC 1679 Max Number of K-Sum Pairs | Two pointers or hash | "pairs summing to k" |
| 4 | LC 1657 Determine if Two Strings Are Close | Frequency counting | "same characters, any arrangement" |
| 5 | LC 649 Dota2 Senate | Queue simulation | round-based elimination |
| 6 | LC 2300 Successful Pairs of Spells and Potions | Sort + binary search | "count how many exceed a threshold" |
| 7 | LC 1926 Nearest Exit from Entrance in Maze | BFS | "nearest" + grid |
| 8 | LC 1372 Longest ZigZag Path in a Binary Tree | Tree DFS carrying state | "longest path" + tree + direction |
| 9 | LC 2462 Total Cost to Hire K Workers | Heap | "k cheapest", from both ends |
| 10 | LC 216 Combination Sum III | Backtracking | "all combinations of exactly k" |
| 11 | LC 1146 Snapshot Array | Design + binary search | versioned reads, sparse writes |
| 12 | LC 2542 Maximum Subsequence Score | Sort + heap | maximise a sum × min product |
| 13 | LC 735 Asteroid Collision | Stack | "most recent survives / collides" |
| 14 | LC 1318 Minimum Flips to Make a OR b Equal to c | Bit manipulation | per-bit independent decisions |
| 15 | LC 2266 Count Number of Texts | 1D DP | "how many ways" + sequence |

### Close (1:50–2:00) — the discrimination round
Five rapid pairs. For each, students say **why it is not** the pattern it resembles:

| Looks like | Actually | Why |
|---|---|---|
| #3 looks like sliding window | two pointers / hash | nothing contiguous is required |
| #7 looks like DFS | BFS | "nearest" demands shortest path |
| #9 looks like sorting | heap | k is small and the set changes as you take |
| #12 looks like DP | greedy + heap | sorting by the min-factor fixes the choice order |
| #15 looks like backtracking | DP | only the **count** is wanted, not the arrangements |

**This drill is the single most concentrated interview-preparation exercise in the course.**

### Flex (2:00–2:30)
Five more problems, same format, chosen by you from whichever patterns the room got wrong.

### Assignment 16.1 (2h)
| Task | Budget |
|---|---|
| **Fully solve any 3 of today's 15**, chosen from patterns you misidentified | 90 min |
| **Written:** for each of the 15, one line — pattern, trigger, complexity. This becomes your personal revision sheet. | 30 min |

---

## Session 2 (Wednesday) — Full Interview Simulation

### Warm-up (0:00–0:10)
Three problems from yesterday, cold: name the pattern in 30 seconds each.

### Format (0:10–1:50)

**Two rounds of 45 minutes**, run as pairs, but with a harder brief than previous mocks:

- **Two problems per round** (one Easy/Medium, one Medium), not one
- **Verbalisation is mandatory throughout** — the interviewer stops the candidate if they go silent for more than 20 seconds
- **No running the code.** Written on a shared screen, verified by walking through an example aloud
- Problems come from **Tuesday's 15** — already classified, so the test is purely execution under pressure

*"Yesterday you said what kind of problem it was. Today you have 22 minutes each to actually do it, out loud, without a compiler."*

### Instructor role
Rotate continuously, scoring against the rubric. **Focus on the two things Mock Round 3 showed to be weakest across the cohort** — for most groups that is testing and complexity justification. Interrupt and ask "what is the complexity, and why?" mid-solution.

### Debrief (1:50–2:00)
Blunt and specific. What would have passed at a real company, and what would not. This is the last chance to change anything before Friday.

### Assignment 16.2 (2h)
| Task | Budget |
|---|---|
| Re-solve both of your problems cleanly, with full complexity justification in the header | 60 min |
| **Red list clearance:** work through your outstanding red-list problems, oldest first | 45 min |
| Update your tracker with a final count of unresolved red-list items | 15 min |

---

## Session 3 (Thursday) — Weakness Clinic & the Roadmap

> **This session is built from your data, not from this document.** Its content depends on Mock Round 3 scores, Checkpoint 2, and red-list patterns across the 14 students.

### Part 1 — Weakness clinic (0:00–1:15)

Before the session, tabulate:
- Mock Round 3 scores by rubric category — where did the cohort lose points?
- Checkpoint 2 by problem — which pattern failed most?
- Red lists — which problems appear on the most students' lists?

Then run **targeted re-teaching**, typically in three parallel groups (feasible with 14 students):

| Group | Common composition | Focus |
|---|---|---|
| **A** | Struggled with DP | Re-derive the five-step procedure; drill state definition on three problems, no coding |
| **B** | Solid technically, weak in the mock | Verbalisation drills — solve while narrating, in pairs, with the instructor interrupting |
| **C** | Strong across the board | Hard problems from patterns they have not been stretched on |

**Rotate between groups.** Group B is usually the largest, and its problem is not knowledge — it is performance.

### Part 2 — The roadmap (1:15–2:00)

What to do after the course ends. Students write this down; it is graded as part of the final weekend set.

**The 90-day plan:**

| Phase | Duration | Activity |
|---|---|---|
| **Consolidate** | Weeks 1–2 after the course | Clear the red list entirely. No new problems until it is empty. |
| **Maintain** | Ongoing | 3–5 problems per week, mixed patterns, always timed. Consistency beats intensity. |
| **Sharpen** | Ongoing | One timed contest every two weeks (LeetCode weekly/biweekly contests are free and well calibrated) |
| **Rehearse** | 4 weeks before interviewing | Two mock interviews per week with a peer. Do not skip this — it is what decays fastest. |

**The revision schedule** — the same spacing that worked all term:
> Re-solve any problem you found hard after **1 week**, then **3 weeks**, then **6 weeks**. If a re-solve takes as long as the original, it is not learned yet.

**Company-specific preparation** — say this honestly:
- Company tags on LeetCode are a reasonable signal but are often stale; do not treat them as a syllabus
- **Frequency matters more than company.** The patterns in this course cover the overwhelming majority of what gets asked anywhere
- Study the **format** rather than the questions: number of rounds, duration, whether code is executed, whether a shared editor is used
- Prepare **two or three of your own projects** to discuss. DSA rounds are not the whole loop

**What this course did not cover, and what to do about it** — being explicit here teaches judgement:

| Not covered | When you will need it | Where to start |
|---|---|---|
| System design | Mid-level and above; some new-grad loops | After you are comfortable in DSA rounds — not before |
| Low-level / OO design | Some product companies | Practise designing a parking lot, an elevator, a card game |
| Behavioural rounds | **Every loop, everywhere** | Write out 6–8 STAR stories now; this is the cheapest points in the whole process |
| Segment trees, MST, flow | Competitive programming, rarely interviews | Only if you take up contests |
| Language internals | Some senior roles | As it comes up |

**The closing message.** Say some version of this directly:

> *"Sixteen weeks ago most of you had not used a hash map. You have now solved around 320 problems, sat ten contests and three mock interview rounds, and several of you can solve a fresh Medium in twenty minutes while explaining it out loud.*
>
> *The single thing that will decide what happens next is whether you keep going. Three to five problems a week is enough to hold this. Stopping for two months is not."*

### Assignment 16.3 (2h)
| Task | Budget |
|---|---|
| Work the material from **your** clinic group | 60 min |
| **Written: your personal 90-day plan** — dated, specific, with your red list attached and named weak patterns | 45 min |
| Final tracker update: total problems solved, contest trend, red list remaining | 15 min |

---

## Session 4 (Friday) — FINAL ASSESSMENT

**Mode:** Checkpoint 3 · **10% of the final grade** · **the exit measurement**

### Format — designed to fit 14 students in one session

- **2 fresh Medium problems, 90 minutes, individual, unlabelled**
- Each submission requires:
  1. Working code
  2. **A 5-minute recorded verbal walkthrough** — approach, why it is correct, complexity — recorded on their own machine and committed
  3. Written complexity with justification
- You observe live, rotating and asking one probing question of each student — score that alongside the artefacts

The recording is what makes the verbal component scale to 14 students in 90 minutes while still being assessed properly.

### Problems
| # | Problem | Difficulty | Pattern (not shown) |
|---|---|---|---|
| 1 | LC 227 Basic Calculator II | Medium | Stack / expression parsing |
| 2 | LC 767 Reorganize String | Medium | Heap + greedy |

Both are fresh, both are frequently asked, and both require a correctness argument rather than a remembered template — LC 767 in particular needs the student to justify *why* always placing the most frequent remaining character works.

### Grading — 25 points
| Component | Points |
|---|---|
| Code review rubric (`01-instructor/03-assessment-and-rubrics.md`, §2) | 15 |
| Verbal walkthrough — clarity, correctness argument, complexity justification | 7 |
| Response to your live probing question | 3 |

### The bar
> **Would this student pass a real phone screen?**

Answer it honestly for each of the 14 and tell them the answer. That single sentence, delivered straight, is the most useful thing they take away from sixteen weeks.

### Assignment 16.4 (2h) — the last one
| Task | Budget |
|---|---|
| Upsolve both assessment problems to a state you would be happy to present | 45 min |
| **Written retrospective:** the three patterns you are strongest in, the three weakest, and what changed most about how you approach an unfamiliar problem | 45 min |
| Ensure your repository is complete and your tracker is final — this is your evidence of 16 weeks of work | 30 min |

---

## Weekend Set 16 (6h) — the final set

No new topics. This set exists to leave students with a clean finish and a working plan.

### Mixed unlabelled set (4h)
**Eight problems, patterns not stated**, drawn from across the whole course and shuffled. Select these yourself from patterns your cohort was weakest in — the Thursday clinic data tells you which.

Suggested composition:
- 2 problems from patterns with the **most red-list entries** across the cohort
- 2 from Phase I (they will be surprised how easy these now feel)
- 2 from the DP families
- 1 graph
- 1 design or backtracking

**Rule: identify the pattern in writing before coding.** The same discipline as Tuesday.

> **The mixed set replaces this week's spaced-revision hour.** Under the standing rule it would draw from weeks 15, 13 and 10; instead the whole set is revision, chosen from wherever the cohort is weakest. A deliberate substitution, not a gap.

### Red list clearance (1h)
Whatever remains. **The target is zero.** Anything still outstanding goes into week 1 of the 90-day plan.

### Written: the 90-day plan, finalised (1h)
Dated, specific, and committed to the repository. It should name:
- The remaining red-list problems and when each will be cleared
- A weekly problem target and which day of the week
- A contest cadence
- A mock-interview partner and a starting date
- The three weakest patterns and how they will be drilled

**Read all 14 of these.** It is the last thing you do for them, and a plan with a date on it is far more likely to survive than an intention.

---

## Instructor notes

### What this week is really for
Students arrive at Week 16 having *done* a great deal and *retained* less than they think. The recognition drill usually reveals that a student who solved 320 problems can still misclassify an unfamiliar one — and that is precisely the failure mode of a real interview. Tuesday is not a review session; it is the most targeted practice of the term.

### Running the clinic well
Three parallel groups with 14 students is genuinely workable and worth the preparation. The instinct to teach everyone the same thing in Week 16 should be resisted — by now their needs have diverged completely, and the data to differentiate is already in your tracker.

### The honest conversation
Some students will not be at the FAANG bar by Friday, and that is a normal outcome from a zero-to-interview-ready sixteen weeks. Tell them where they are and what specifically closes the gap — a named list of patterns and a timeline. **A concrete "you are three months of consistent practice away, and here is what to practise" is far more useful, and more respectful, than vague encouragement.**

### What to cut if you are behind
Very little should be cut this week — none of it is new material and all of it is high-yield. If you must:
1. Tuesday's flex (the extra five problems)
2. The weekend mixed set from 8 problems to 6

**Never cut:** the recognition drill, the roadmap, the 90-day plan, or the honest individual conversation after the final assessment.
