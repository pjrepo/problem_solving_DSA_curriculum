# Week 14 — DP II · Optional Practice

**Phase III** · Week band: **Medium/Hard** · Reinforce = Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Six DP families across Weeks 13 and 14. The point of naming them is that "it's a DP problem" is not a plan — "it's a knapsack" is. Use the Reinforce track here to get each family's *state* automatic, because that is the part interviews test.
>
> Reinforce is **Medium** this week, per the band.

---

## Session 1 (Tuesday) — The Knapsack Family

> Optional. Not graded. Skip freely.

### Check your understanding
1. 0/1 knapsack versus unbounded: what single change in the loop distinguishes them, and why does that change work?
2. Coin Change asks for the fewest coins; Coin Change II asks how many ways. Which loop order changes, and what goes wrong if you use the other one?
3. Partition Equal Subset Sum is knapsack in disguise. What is the capacity, and what are the item weights?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1155 Number of Dice Rolls With Target Sum | Medium | 35 min |
| LC 650 2 Keys Keyboard | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 376 Wiggle Subsequence | Medium | 40 min |
| LC 132 Palindrome Partitioning II | **Hard** | 55 min |

### 3. Revision — Week 13 (DP I) · Week 11 (Graphs I) · Week 8 (Binary Search & Sorting)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 922 Sort Array By Parity II | Easy | 25 min |
| LC 909 Snakes and Ladders | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why does the inner loop run backwards in the 0/1 version?"** — A good answer says it stops an item being reused within the same pass, because a forward loop would read a value already updated for this item. This is the single most-asked knapsack follow-up.
2. **"What is the complexity, and is it polynomial?"** — A good answer gives O(n × capacity) and knows to call it *pseudo*-polynomial, because capacity is a value rather than an input length. Saying that unprompted is a genuinely strong signal.

---

## Session 2 (Wednesday) — String / Two-Sequence DP

> Optional. Not graded. Skip freely.

### Check your understanding
1. Two-sequence DP: what do the two indices mean, and what does `dp[i][j]` represent?
2. Edit Distance has three transitions. Name them, and say which cell each one reads.
3. Longest Common Subsequence and Longest Common Substring differ by one rule. What is it?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 166 Fraction to Recurring Decimal | Medium | 35 min |
| LC 606 Construct String from Binary Tree | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1035 Uncrossed Lines | Medium | 40 min |
| LC 1235 Maximum Profit in Job Scheduling | **Hard** | 55 min |

LC 1035 is Longest Common Subsequence with the geometry stripped away — recognising that is the entire problem, and it is a good test of whether the family has landed.

### 3. Revision — Week 13 (DP I) · Week 11 (Graphs I) · Week 8 (Binary Search & Sorting)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1502 Can Make Arithmetic Progression From Sequence | Easy | 25 min |
| LC 429 N-ary Tree Level Order Traversal | Medium | 35 min |

### 4. Interview follow-ups
1. **"Your table is (m+1) × (n+1). Why the extra row and column?"** — A good answer says they encode the empty-prefix base cases, which removes a pile of special-casing. Being able to explain the padding rather than copying it is the difference here.
2. **"Reconstruct the actual subsequence, not just its length."** — A good answer walks backwards through the table following the choices that produced each value. Most candidates have only ever computed the length, so this is a strong differentiator.

---

## Session 3 (Thursday) — State Machine DP & Bitmask (awareness)

> Optional. Not graded. Skip freely.

### Check your understanding
1. State-machine DP: for the stock problems, what are the states, and what are the legal transitions between them?
2. Best Time to Buy and Sell Stock with a cooldown — what does the extra state represent?
3. Bitmask DP is awareness-level in this course. What does a bit in the mask *mean*, and what input size makes it viable at all?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1653 Minimum Deletions to Make String Balanced | Medium | 35 min |
| LC 790 Domino and Tromino Tiling | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1976 Number of Ways to Arrive at Destination | Medium | 40 min |
| LC 403 Frog Jump | **Hard** | 55 min |

LC 403 is DP where the state includes the last jump size — a good example of a state that is not just "where am I".

### 3. Revision — Week 13 (DP I) · Week 11 (Graphs I) · Week 8 (Binary Search & Sorting)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1636 Sort Array by Increasing Frequency | Easy | 25 min |
| LC 456 132 Pattern | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why is this a state machine rather than a 1D DP?"** — A good answer says the answer at each index depends on which *mode* you are in — holding, sold, cooling down — so the index alone is not a sufficient state. That is the family's defining property.
2. **"The constraint says n ≤ 20. What does that tell you?"** — A good answer goes straight to exponential: 2²⁰ is about a million, so a bitmask over subsets is intended. This is the Week 1 constraints table paying off in Week 14.

---

## Session 4 (Friday) — ARENA: Contest 9

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Name all six DP families and give one canonical problem for each. From memory.
2. For each contest problem: which family was it, and did you identify the family before or after you started coding?
3. Which of the six do you least trust? That is your Week 16 revision target.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 792 Number of Matching Subsequences | Medium | 35 min |
| LC 2140 Solving Questions With Brainpower | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 799 Champagne Tower | Medium | 40 min |
| LC 1092 Shortest Common Supersequence | **Hard** | 55 min |

LC 1092 combines Longest Common Subsequence with a reconstruction step — the two halves of Wednesday's session in one Hard.

### 3. Revision — Week 13 (DP I) · Week 11 (Graphs I) · Week 8 (Binary Search & Sorting)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 747 Largest Number At Least Twice of Others | Easy | 25 min |
| LC 958 Check Completeness of a Binary Tree | Medium | 35 min |

### 4. Interview follow-ups
1. **"Which DP family is this, and how did you decide?"** — A good answer names the family from the *question shape* — subsets summing to a target, two sequences aligned, a mode that changes over time — rather than from the surface story. That is the whole reason the families are taught by name.
2. **"You have a working memoised solution. Would you convert it to a table?"** — A good answer says what conversion buys — no recursion depth risk, sometimes better constants — and what it costs in clarity, then makes a recommendation instead of hedging.
