# Week 3 — Two Pointers & Sliding Window · Optional Practice

**Phase I** · Week band: **Easy/Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> This is the highest-yield week in Phase I. If you only ever use this file once, use it here.

---

## Session 1 (Tuesday) — Two Pointers: Opposite Ends

> Optional. Not graded. Skip freely.

### Check your understanding
1. Two pointers from opposite ends requires something of the input. What, and why does the technique collapse without it?
2. In Container With Most Water, you move the pointer at the *shorter* wall. Argue why moving the taller one can never help.
3. What is the loop condition — `left < right` or `left <= right`? Give a problem where each is correct.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 345 Reverse Vowels of a String | Easy | 20 min |
| LC 917 Reverse Only Letters | Easy | 20 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 923 3Sum With Multiplicity | Medium | 40 min |
| LC 407 Trapping Rain Water II | **Hard** | 60 min |

LC 923 is 3Sum where the counting, not the finding, is the difficulty. LC 407 is the 2D version of the problem that shows up in Friday's contest — it needs a heap, which is Week 10, so read it as a preview rather than a target.

### 3. Revision — Week 2: prefix sums, Kadane's, counting
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1991 Find the Middle Index in Array | Easy | 25 min |
| LC 1314 Matrix Block Sum | Medium | 35 min |

### 4. Interview follow-ups
1. **"What if the input were not sorted?"** — A good answer prices the fix: sort first for O(n log n), or use a hash map for O(n) time and O(n) space. Then it *asks* which the interviewer prefers rather than guessing.
2. **"Your solution skips duplicates. Show me exactly where, and why it is correct."** — A good answer points at the line and argues that skipping cannot lose a distinct triple. Duplicate handling is where most 3Sum answers actually fail.

---

## Session 2 (Wednesday) — Two Pointers: Same Direction & Partitioning

> Optional. Not graded. Skip freely.

### Check your understanding
1. In the read/write two-pointer pattern, what does the *write* pointer actually mean at any moment? State it as an invariant.
2. Sort Colors in one pass uses three pointers. Name what each one guarantees about the region behind it.
3. Why can the fast/slow read-write pattern always be done in place, while the opposite-end pattern sometimes cannot?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1089 Duplicate Zeros | Easy | 30 min |
| LC 844 Backspace String Compare | Easy | 30 min |

LC 1089 is worth doing specifically because the obvious forward pass is wrong; going backwards is the fix.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2161 Partition Array According to Given Pivot | Medium | 35 min |
| LC 135 Candy | **Hard** | 55 min |

LC 135 is solved by two directional passes — left to right, then right to left — and it is the cleanest argument in the course for why direction matters. If you want more after it, the week's own contest P4 (LC 42) is the same shape.

### 3. Revision — Week 2: counting and prefix sums
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1002 Find Common Characters | Easy | 25 min |
| LC 1413 Minimum Value to Get Positive Step by Step Sum | Easy | 20 min |

### 4. Interview follow-ups
1. **"You modified the input. Is that acceptable?"** — A good answer asks rather than assumes, and can state the O(1)-space version and the copy-first version with their costs.
2. **"Can you do it without the extra array?"** — A good answer identifies whether the algorithm reads a position after it has written past it. If it does, in-place needs a direction change; if not, in-place is free.

---

## Session 3 (Thursday) — Sliding Window

> Optional. Not graded. Skip freely.

### Check your understanding
1. Fixed-size window versus variable-size window: what triggers the shrink in each?
2. Write the four lines of the variable-window template from memory: expand, update state, shrink while invalid, record answer. Which order matters?
3. Why is a sliding window O(n) and not O(n²), given the inner `while` loop?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2379 Minimum Recolors to Get K Consecutive Black Blocks | Easy | 25 min |
| LC 1876 Substrings of Size Three with Distinct Characters | Easy | 20 min |

Both are fixed-size windows — the easier half of today. Get these automatic before attempting variable windows.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1695 Maximum Erasure Value | Medium | 40 min |
| LC 992 Subarrays with K Different Integers | **Hard** | 60 min |

LC 992 is the problem that teaches "exactly K = at most K minus at most K−1". That trick reappears constantly and is worth the hour even if you need the editorial.

### 3. Revision — Week 2: prefix sums and counting
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1422 Maximum Score After Splitting a String | Easy | 25 min |
| LC 1013 Partition Array Into Three Parts With Equal Sum | Easy | 30 min |

### 4. Interview follow-ups
1. **"Why is the amortised cost O(n) when there is a nested loop?"** — A good answer says each pointer only ever moves forward, so across the whole run there are at most 2n pointer moves. This exact question is asked constantly and answering it well is a strong signal.
2. **"What if the characters were not limited to lowercase letters?"** — A good answer notes that a 26-slot array becomes a hash map, that the complexity is unchanged in big-O terms, and that the constant factor gets worse.

---

## Session 4 (Friday) — ARENA: Contest 2

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Two pointers or sliding window? Give the signal in the problem statement that decides it.
2. Which of this week's four contest problems would you now solve in under 15 minutes? Which would still take 40?
3. From memory: the variable-window template, complete, in a blank file. Time yourself.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2269 Find the K-Beauty of a Number | Easy | 20 min |
| LC 942 DI String Match | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1838 Frequency of the Most Frequent Element | Medium | 45 min |
| LC 995 Minimum Number of K Consecutive Bit Flips | **Hard** | 60 min |

LC 1838 is a sliding window whose validity test needs a prefix sum — the two ideas from Weeks 2 and 3 in one problem. LC 995 pairs a window with a difference array, and it is the best argument in the course for why Week 2 mattered.

### 3. Revision — the whole of Week 2
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1550 Three Consecutive Odds | Easy | 20 min |
| LC 930 Binary Subarrays With Sum | Medium | 40 min |

### 4. Interview follow-ups
1. **"You have a working O(n²). Talk me to O(n)."** — A good answer narrates the waste first: "I am re-scanning a window I already scanned." Naming the waste is what earns the hint if you need one.
2. **"Which pointer do you move, and how do you know you have not missed an answer?"** — A good answer gives the invariant, not the code. If you cannot state what is true of everything behind your pointers, you do not yet have the solution — you have a guess that passes.
