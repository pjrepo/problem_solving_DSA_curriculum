# Week 8 — Binary Search & Sorting · Optional Practice

**Phase II** · Week band: **Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> If Week 7 was rough, this week is the recovery: binary search is mechanical in a way recursion is not. Use the Reinforce track freely — volume genuinely helps here, because the difficulty is entirely in getting the boundaries right.

---

## Session 1 (Tuesday) — Binary Search on Arrays

> Optional. Not graded. Skip freely.

### Check your understanding
1. Write the binary search template from memory. Is your loop `while lo < hi` or `while lo <= hi`, and what does `lo` mean when the loop ends?
2. Why is `mid = lo + (hi - lo) // 2` preferred over `(lo + hi) // 2`?
3. The target is absent. Where does `lo` end up, and why is that the useful answer to "where would it be inserted?"

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 69 Sqrt(x) | Easy | 20 min |
| LC 374 Guess Number Higher or Lower | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 540 Single Element in a Sorted Array | Medium | 40 min |
| LC 154 Find Minimum in Rotated Sorted Array II | **Hard** | 55 min |

### 3. Revision — Week 7 (Recursion) · Week 5 (Linked Lists) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1189 Maximum Number of Balloons | Easy | 25 min |
| LC 981 Time Based Key-Value Store | Medium | 35 min |

### 4. Interview follow-ups
1. **"Your loop terminates. Prove it."** — A good answer shows the search space strictly shrinks every iteration — and identifies the exact line that guarantees it. Infinite loops in binary search always come from an update that fails to shrink the range.
2. **"How would you find the *first* occurrence rather than any occurrence?"** — A good answer keeps searching left after a match instead of returning, and states the new invariant. This one modification covers a large share of binary search interview questions.

---

## Session 2 (Wednesday) — Binary Search on the Answer Space

> Optional. Not graded. Skip freely.

### Check your understanding
1. "Binary search on the answer" — what are you searching over, if not the array?
2. The technique needs a predicate. What property must that predicate have for binary search to be valid?
3. Koko Eating Bananas: what is the search space, what is the predicate, and what is the complexity including the check?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 367 Valid Perfect Square | Easy | 20 min |
| LC 1539 Kth Missing Positive Number | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 852 Peak Index in a Mountain Array | Medium | 40 min |
| LC 719 Find K-th Smallest Pair Distance | **Hard** | 55 min |

LC 719 is binary search on the answer *plus* a two-pointer counting step — the two techniques composed. It is the best single problem for seeing that binary search is a search over any monotone space.

### 3. Revision — Week 7 (Recursion) · Week 5 (Linked Lists) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 3005 Count Elements With Maximum Frequency | Easy | 25 min |
| LC 528 Random Pick with Weight | Medium | 35 min |

### 4. Interview follow-ups
1. **"How do you know the predicate is monotonic?"** — A good answer argues it directly: if a capacity of 5 works then 6 must also work, because more capacity never hurts. Stating that argument out loud is what justifies using binary search at all.
2. **"What are the bounds of your search space, and why those?"** — A good answer justifies both ends — the smallest and largest conceivable answer — and notes that a wrong lower bound is the most common bug in this pattern.

---

## Session 3 (Thursday) — Sorting as a Tool & Intervals

> Optional. Not graded. Skip freely.

### Check your understanding
1. "Sort first" is a legitimate technique. What does sorting cost you, and what does it buy?
2. For merging intervals, what do you sort by, and why is sorting by end time the right choice for a *different* problem?
3. The exchange argument: how do you convince someone a greedy choice is safe?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 860 Lemonade Change | Easy | 20 min |
| LC 1200 Minimum Absolute Difference | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 334 Increasing Triplet Subsequence | Medium | 40 min |
| LC 330 Patching Array | **Hard** | 55 min |

### 3. Revision — Week 7 (Recursion) · Week 5 (Linked Lists) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1704 Determine if String Halves Are Alike | Easy | 25 min |
| LC 1769 Minimum Number of Operations to Move All Balls to Each Box | Medium | 35 min |

### 4. Interview follow-ups
1. **"You sorted by start time. Would sorting by end time also work?"** — A good answer says which problem each sort key solves — start time for merging, end time for maximum non-overlapping selection — and explains why. Getting this backwards is the classic interval mistake.
2. **"Prove your greedy choice is optimal."** — A good answer gives an exchange argument: assume an optimal solution differs from the greedy choice, then show you can swap in the greedy choice without making things worse. Handwaving "it's greedy so it works" scores badly.

---

## Session 4 (Friday) — ARENA: Contest 5

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Which is it: binary search on the array, or binary search on the answer? Name the signal in the problem statement that tells you.
2. From memory, in a blank file: the binary search template, then the binary-search-on-answer template. Both, timed.
3. For each contest problem: was your first instinct the right pattern? If not, what misled you?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 441 Arranging Coins | Easy | 20 min |
| LC 2540 Minimum Common Value | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1283 Find the Smallest Divisor Given a Threshold | Medium | 40 min |
| LC 1095 Find in Mountain Array | **Hard** | 55 min |

LC 1095 hides the array behind an API, which forces you to think about the *number of queries* as the cost — a good reminder that complexity is about operations, not lines.

### 3. Revision — Week 7 (Recursion) · Week 5 (Linked Lists) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1221 Split a String in Balanced Strings | Easy | 25 min |
| LC 2225 Find Players With Zero or One Losses | Medium | 35 min |

### 4. Interview follow-ups
1. **"The array is rotated. Does your binary search still work?"** — A good answer says no, not unmodified, and describes the extra decision: work out which half is sorted, then decide whether the target lies in it. This is the most-asked binary search follow-up there is.
2. **"What if there are duplicates?"** — A good answer notes that duplicates can break the "which half is sorted" test and push the worst case to O(n) — and that LC 81 in this week's standard set exists precisely to make that point.
