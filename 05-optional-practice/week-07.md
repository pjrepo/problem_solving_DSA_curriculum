# Week 7 — Recursion · Optional Practice

**Phase II** · Week band: **Medium** · Reinforce = Easy/Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> ## ⚠ Read this before opening anything else on this page
>
> **This is the hardest week of the course, and it is the week you should most likely ignore this file entirely.**
>
> Week 7 gates Weeks 9, 13, 14 and 15. If recursion has not clicked yet, more problems is the wrong medicine — the right one is re-tracing the standard set by hand, on paper, until the call stack is a thing you can see. The week file ends with an explicit go/no-go decision for exactly this reason.
>
> Open this file **only** if the standard set landed comfortably. If it did not, close it and go and draw a recursion tree.

---

## Session 1 (Tuesday) — The Mental Model

> Optional. Not graded. Skip freely.

### Check your understanding
1. Every recursive function needs two things. Name them, and say what happens if the second is wrong rather than missing.
2. Trace `f(4)` for Fibonacci by hand. How many times is `f(2)` computed, and what does that tell you about the complexity?
3. "Do the small thing, then trust the function for the rest." What is the small thing in reversing a linked list recursively?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 326 Power of Three | Easy | 20 min |
| LC 2094 Finding 3-Digit Even Numbers | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1823 Find the Winner of the Circular Game | Medium | 40 min |
| LC 224 Basic Calculator | **Hard** | 55 min |

LC 1823 is the Josephus problem — the recursive formulation is four lines and genuinely hard to see. LC 224 is a recursive-descent parser and the best possible use of a stack-plus-recursion mental model.

### 3. Revision — Week 6 (Stacks & Queues) · Week 4 (Strings & Matrices) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1903 Largest Odd Number in String | Easy | 25 min |
| LC 316 Remove Duplicate Letters | Medium | 35 min |

### 4. Interview follow-ups
1. **"What is the space complexity of your recursive solution?"** — A good answer counts the maximum call-stack depth, not the number of calls. For a balanced recursion that is O(log n); for a linear one, O(n). Candidates claiming O(1) for recursive code are caught here immediately.
2. **"Could you write this iteratively?"** — A good answer says what the explicit stack would hold — precisely the local state each frame carries — and whether the trade is worth it. Week 6's stack session was the preparation for this answer.

---

## Session 2 (Wednesday) — Recursion Trees & Complexity

> Optional. Not graded. Skip freely.

### Check your understanding
1. Draw the recursion tree for a function that calls itself twice with `n/2`. How many nodes, how deep, and what is the total work?
2. Now one that calls itself twice with `n − 1`. What changed, and why is that the difference between usable and useless?
3. Where in the recursion tree does memoisation actually save you work? Point at the nodes.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1922 Count Good Numbers | Medium | 35 min |
| LC 2487 Remove Nodes From Linked List | Medium | 40 min |

Both are gentle Mediums — the Easy recursion problems are almost all in the standard set. If these feel hard, that is the signal described in the warning at the top of this file.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1545 Find Kth Bit in Nth Binary String | Medium | 40 min |
| LC 60 Permutation Sequence | **Hard** | 55 min |

### 3. Revision — Week 6 (Stacks & Queues) · Week 4 (Strings & Matrices) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 520 Detect Capital | Easy | 25 min |
| LC 846 Hand of Straights | Medium | 35 min |

### 4. Interview follow-ups
1. **"How many times is the same subproblem solved?"** — A good answer counts repeated nodes in the recursion tree and says that the count *is* the argument for memoisation. This is the exact conversation Week 13 opens with, so having it now is free progress.
2. **"Your recursion is 40 levels deep and the language limit is 1000. Is that fine?"** — A good answer notes that depth is bounded by the input, states the bound, and says when the language's own recursion limit becomes a real engineering constraint rather than a theoretical one.

---

## Session 3 (Thursday) — Divide & Conquer

> Optional. Not graded. Skip freely.

### Check your understanding
1. Divide and conquer has three steps. Name them, and say which one merge sort spends its time in.
2. Merge sort is O(n log n). Where does the log come from, and where does the n come from?
3. Quickselect averages O(n) while a full sort is O(n log n). What work does quickselect deliberately not do?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 324 Wiggle Sort II | Medium | 35 min |
| LC 372 Super Pow | Medium | 40 min |

Gentle Mediums again, for the same reason as Wednesday.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 932 Beautiful Array | Medium | 40 min |
| LC 761 Special Binary String | **Hard** | 55 min |

### 3. Revision — Week 6 (Stacks & Queues) · Week 4 (Strings & Matrices) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 929 Unique Email Addresses | Easy | 25 min |
| LC 1209 Remove All Adjacent Duplicates in String II | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why is quickselect O(n) on average but O(n²) in the worst case?"** — A good answer describes the partition sizes in each case and names the fix — random pivot choice. Being able to say *why* the worst case happens, not just that it exists, is the difference here.
2. **"Merge sort needs O(n) extra space. Can you avoid it?"** — A good answer says in-place merging is possible but complicated and slower in practice, and that on a linked list merge sort is naturally O(1) extra — which is why Week 5's LC 148 exists.

---

## Session 4 (Friday) — ARENA: Contest 4

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. From memory, in a blank file: merge sort, complete. Then quickselect. This is the single most valuable 40 minutes available to you this week.
2. Which contest problem did you solve recursively that would have been easier iteratively — or the reverse?
3. **The honest question.** Can you now write a recursive solution to an unfamiliar problem without copying the shape of one you have seen? If not, say so to your professor this week rather than in Week 13.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 342 Power of Four | Easy | 20 min |
| LC 390 Elimination Game | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 894 All Possible Full Binary Trees | Medium | 40 min |
| LC 273 Integer to English Words | **Hard** | 55 min |

LC 894 builds *structures* recursively rather than computing values — the shape Week 15's backtracking uses. LC 273 is pure case analysis and a good palate cleanser.

### 3. Revision — Week 6 (Stacks & Queues) · Week 4 (Strings & Matrices) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1832 Check if the Sentence Is Pangram | Easy | 25 min |
| LC 1910 Remove All Occurrences of a Substring | Medium | 35 min |

### 4. Interview follow-ups
1. **"Talk me through your base case."** — A good answer states the smallest input and what the function returns for it, then argues that every recursive call moves strictly towards it. That termination argument is what interviewers are actually checking.
2. **"Would memoising this help?"** — A good answer checks whether subproblems actually repeat before saying yes. Memoising a recursion whose subproblems are all distinct adds space for nothing — recognising that is worth more than reflexively reaching for a cache.
