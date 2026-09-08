# Week 14 — Dynamic Programming II: Knapsack, Strings & State Machines

**Phase III: Integration** · Difficulty band: **Medium/Hard**

> The remaining three DP families. **Knapsack** is where the state gains a second dimension; **string DP** is where `dp[i][j]` spans two sequences; **state machines** are where the state is *a situation you are in* rather than a position in an array.
>
> Watch morale. This is the sixth consecutive session of the hardest topic in the course, and DP fatigue is real. The contest is calibrated accordingly.

---

## Exit criteria

- [ ] Distinguish 0/1 from unbounded knapsack, and say **which loop order produces which**
- [ ] Recognise "can a subset reach exactly this total?" as subset-sum
- [ ] Write two-sequence DP with `dp[i][j]` and correct boundary rows
- [ ] Explain the match/mismatch decision in LCS and Edit Distance
- [ ] Model a problem as a state machine and draw its transition diagram
- [ ] Explain what a bitmask represents in DP (awareness level)

---

## Session 1 (Tuesday) — The Knapsack Family

### Weekend-set debrief (0:00–0:15)
Debrief **LC 322 Coin Change** editorials — look specifically for the phrase *"candidate for the last coin."* Students who found that framing are ready for this week; those who did not need close attention today. Then **LC 918**, and its callback to Week 2's Kadane.

### Concept spine (0:15–0:45)

**Part A — the setup.**

> You have items, each with a weight and a value, and a bag of limited capacity. Maximise the value you carry.

**Two variants, and the difference matters:**
- **0/1 knapsack** — each item may be taken **at most once**
- **Unbounded knapsack** — each item may be taken **any number of times**

**State:** `dp[i][c]` = the best value using the first `i` items with capacity `c`.

**Recurrence:** for each item, take it or don't.
```
dp[i][c] = max( dp[i-1][c],                          # skip item i
                dp[i-1][c - w[i]] + v[i] )           # take item i
```

**Part B — the loop-order rule, which is the week's most practical fact.**

Compressed to one dimension, the *direction* of the capacity loop decides the variant:

```python
# 0/1 knapsack — capacity loop goes BACKWARD
for item in items:
    for c in range(capacity, item.w - 1, -1):
        dp[c] = max(dp[c], dp[c - item.w] + item.v)

# Unbounded knapsack — capacity loop goes FORWARD
for item in items:
    for c in range(item.w, capacity + 1):
        dp[c] = max(dp[c], dp[c - item.w] + item.v)
```

*"Why?"* Let them work it out.

> **Going forward, `dp[c - w]` may already include this same item — so you can reuse it. Going backward, `dp[c - w]` is still from the previous item's row — so each item is used at most once.**

**This one detail separates LC 322 (Coin Change, unbounded) from LC 416 (Partition, 0/1)**, and it is a classic interview probe. Make students state the reason, not the rule.

**Part C — subset sum, the disguise students must learn to see.**

> **LC 416 Partition Equal Subset Sum.** Can an array be split into two parts with equal sums?

*"If the total is odd, no. Otherwise each part must total half — so: **can some subset sum to exactly `total/2`?** That is 0/1 knapsack where weight equals value and we ask about exact achievability."*

State: `dp[c]` = *can a subset reach exactly total `c`?* Boolean, not numeric.

**The recognition signal to name:** *"'Can a subset reach exactly X' is always knapsack. So is 'split into two equal groups', 'reach a target with + and −', and 'minimise the difference between two groups.'"*

**Part D — LC 494 Target Sum.** Assign `+` or `−` to each number to reach a target. Looks nothing like knapsack.

> Let `P` be the positives and `N` the negatives. `P − N = target` and `P + N = total`, so `P = (target + total) / 2`. **That is a subset-sum for `P`.**

*"The algebra is the whole problem. Once you have it, you have already written the code."*

### Live-code (0:45–1:10)
**LC 416** with the 1D backward loop. Then **LC 518 Coin Change II** beside **LC 322** from last week, with only the loop direction differing — the clearest possible demonstration of Part B.

### Guided practice (1:15–1:50)
1. **LC 416 Partition Equal Subset Sum** — Medium
2. **LC 494 Target Sum** — Medium
3. **LC 518 Coin Change II** — Medium
4. **LC 1049 Last Stone Weight II** — Medium. Minimising the difference between two groups — the same reduction as LC 416 wearing a different costume.

### Live critique (1:50–2:00)
An *LC 518*. Focus: which loop is outer? Swapping the item and capacity loops in Coin Change II counts **permutations** instead of **combinations** and gives the wrong answer. Ask the author to explain the difference — this is subtle, important, and frequently asked.

### Flex (2:00–2:30)
**LC 474 Ones and Zeroes** — Medium. Knapsack with **two** capacity dimensions. `dp[zeros][ones]`; the extension is natural once one dimension is solid.

### Common misconceptions
- Wrong loop direction, silently producing the other variant.
- In LC 518, item and capacity loops swapped.
- Forgetting the odd-total early exit in LC 416.
- Trying to reconstruct *which* items were chosen without storing decisions.

### Assignment 14.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 416 Partition Equal Subset Sum | Medium | 35 min |
| LC 494 Target Sum | Medium | 35 min |
| LC 518 Coin Change II | Medium | 25 min |
| LC 1049 Last Stone Weight II | Medium | 35 min |

---

## Session 2 (Wednesday) — String / Two-Sequence DP

### Warm-up (0:00–0:10)
1. Forward or backward capacity loop for 0/1 knapsack, and why?
2. What is the reduction in LC 494?
3. What does `dp[c]` mean in LC 416?

### Concept spine (0:10–0:45)

**Part A — the shape.** Two strings, so the state has one index into each.

> **`dp[i][j]` = the answer for the first `i` characters of A and the first `j` characters of B.**

Every problem in this family answers one question at each cell: **do `A[i-1]` and `B[j-1]` match, and what follows from that?**

**Part B — LCS (LC 1143), derived.**

```
if A[i-1] == B[j-1]:  dp[i][j] = 1 + dp[i-1][j-1]        # both consumed, +1
else:                 dp[i][j] = max(dp[i-1][j], dp[i][j-1])   # drop one or the other
```

**Draw the grid on the board** with `"abcde"` and `"ace"` and fill it in with the class. The table makes the recurrence obvious in a way the formula never does.

**The 1-indexing convention:** `dp` has dimensions `(m+1) × (n+1)`, and row/column 0 means "empty string". This makes base cases trivial (all zeros) and removes every boundary special case. **State the convention explicitly and use it consistently** — mixing conventions is where the bugs live.

**Part C — Edit Distance (LC 72).** Same skeleton, three operations instead of two.

```
if A[i-1] == B[j-1]:  dp[i][j] = dp[i-1][j-1]            # free, no operation
else:                 dp[i][j] = 1 + min(dp[i-1][j-1],   # replace
                                         dp[i-1][j],     # delete from A
                                         dp[i][j-1])     # insert into A
```

**Name each of the three, pointing at the cell it comes from.** Students who can say "that one is a deletion" have understood; students reciting `min` of three cells have not.

Base cases carry real meaning here: `dp[i][0] = i` (delete everything), `dp[0][j] = j` (insert everything).

**Part D — the palindrome trick.**

> **LC 516 Longest Palindromic Subsequence** = **LCS of the string with its own reverse.**

Let the room sit with that for a moment. It is a genuinely delightful reduction, and it teaches the habit of asking *"is this a problem I already solved, in disguise?"*

### Live-code (0:45–1:10)
**LC 1143 LCS** with the grid drawn and filled alongside the code, then **LC 72 Edit Distance** with the three operations named.

### Guided practice (1:15–1:50)
1. **LC 1143 Longest Common Subsequence** — Medium
2. **LC 72 Edit Distance** — Medium
3. **LC 516 Longest Palindromic Subsequence** — Medium
4. **LC 583 Delete Operation for Two Strings** — Medium. It is `m + n − 2·LCS`; recognising that is the point.

### Live critique (1:50–2:00)
An *LC 72*. Focus: are the base rows and columns correct, and can the author name each of the three transitions? Ask them to point at the cell each one comes from.

### Flex (2:00–2:30)
**LC 5 / LC 647 re-solved as interval DP.** In Week 4 they used expand-around-centre. Now: `dp[i][j]` = is `s[i..j]` a palindrome? — with the recurrence `s[i] == s[j] and dp[i+1][j-1]`. **Note the fill order must be by increasing substring length**, since `dp[i][j]` depends on a *shorter* interval. That ordering requirement is the introduction to interval DP.

### Common misconceptions
- Mixing 0-indexed and 1-indexed conventions within one solution.
- Wrong base row/column in Edit Distance (zeros instead of `i` and `j`).
- Confusing subsequence (non-contiguous) with substring (contiguous). **Define both again — it still catches people.**
- In interval DP, filling by row instead of by increasing length.

### Assignment 14.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1143 Longest Common Subsequence | Medium | 30 min |
| LC 72 Edit Distance | Medium | 40 min |
| LC 516 Longest Palindromic Subsequence | Medium | 25 min |
| LC 583 Delete Operation for Two Strings | Medium | 25 min |

---

## Session 3 (Thursday) — State Machine DP & Bitmask (awareness)

### Warm-up (0:00–0:10)
1. What does `dp[i][j]` mean in LCS?
2. Name Edit Distance's three transitions and the cell each comes from.
3. LC 516 reduces to what?

### Concept spine (0:10–0:45)

**Part A — when the state is a situation.**

Every DP so far has had a state that was a *position*. Sometimes the state is **which situation you are in**.

> **Stock trading.** At any day you are either **holding** a stock or **not holding** one. Those are your states, and the days are your timeline.

**Draw the transition diagram on the board:**

```
        ┌──── buy (−price) ────┐
        ↓                      │
   [ HOLDING ]            [ NOT HOLDING ]
        │                      ↑
        └──── sell (+price) ───┘
```

```python
def max_profit(prices):
    hold, free = float('-inf'), 0        # hold = best profit while holding
    for p in prices:                     # free = best profit while not holding
        hold, free = max(hold, free - p), max(free, hold + p)
    return free
```

**Two lines of state, and the whole family falls out of it:**

| Problem | Change to the machine |
|---|---|
| LC 121 (one transaction) | `hold = max(hold, -p)` — cannot re-invest previous profit |
| LC 122 (unlimited) | the base machine above |
| LC 309 (cooldown) | add a **cooldown** state between selling and buying again |
| LC 714 (fee) | subtract the fee on the sell transition |
| LC 123 / LC 188 (at most k) | add a transaction-count dimension |

> ***"Five problems that look different are one machine with different transitions. Draw the diagram before you write code — the code is a direct transcription of the diagram."***

Students met LC 121 in Week 1 and LC 122 in Week 2. Showing that both were secretly this machine is one of the most satisfying moments of the course.

**Part B — bitmask DP (awareness only).**

> When the state is *"which subset of a small set have I already used?"*, encode that subset as the bits of an integer. `mask = 5` is binary `101`, meaning items 0 and 2 are used.

Useful operations:
```python
mask | (1 << i)          # add item i
mask & (1 << i)          # is item i present?
bin(mask).count('1')     # how many are used
```

*"Feasible only for about 20 items or fewer, since there are 2ⁿ masks. It is the standard tool for travelling-salesman-shaped problems."*

**Say the scope boundary explicitly:** *"You should recognise this and be able to explain the idea. Implementing bitmask DP under time pressure is not something interviewers at product companies expect, so we are not drilling it."* Naming what you are *not* teaching, and why, teaches judgement.

### Live-code (0:45–1:10)
The two-state machine, then **LC 309 (cooldown)** by adding a third state to the diagram before touching code.

### Guided practice (1:15–1:50)
1. **LC 122 Best Time to Buy and Sell Stock II** — Medium. Re-solved as a state machine (they solved it greedily in Week 2).
2. **LC 309 Best Time to Buy and Sell Stock with Cooldown** — Medium
3. **LC 714 Best Time to Buy and Sell Stock with Transaction Fee** — Medium
4. **LC 123 Best Time to Buy and Sell Stock III** — Hard. At most two transactions; four states.

### Live critique (1:50–2:00)
An *LC 309*. Focus: **ask the author to draw their state machine on the board.** If they cannot, the code was copied. This is the fastest possible check for understanding in this family.

### Flex (2:00–2:30)
Bitmask exploration, no implementation required: enumerate all subsets of `[1,2,3]` using masks 0–7, and discuss how a TSP state `(mask, current_city)` would work. Awareness level, as scoped.

### Common misconceptions
- Updating `hold` and `free` sequentially rather than simultaneously — using the new `hold` when computing `free` in the same step. **The tuple assignment is what prevents this**; point at it.
- Initialising `hold = 0` instead of `-inf`, allowing a sell before any buy.
- In LC 309, applying the cooldown to buying rather than to the state after selling.

### Assignment 14.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 122 Best Time to Buy and Sell Stock II (state machine version) | Medium | 25 min |
| LC 309 Best Time to Buy and Sell Stock with Cooldown | Medium | 35 min |
| LC 714 Best Time with Transaction Fee | Medium | 25 min |
| LC 123 Best Time to Buy and Sell Stock III | Hard | 45 min |

**Every solution must include the state machine diagram as an ASCII comment.** Graded — it is faster to check than the code and far more diagnostic.

---

## Session 4 (Friday) — ARENA: Contest 9

**Mode:** Contest · 90 minutes · **the final contest of the course**

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 1025 Divisor Game | Easy | Simple DP (with a one-line insight for those who spot it) |
| P2 | LC 377 Combination Sum IV | Medium | Unbounded knapsack where **order matters** — contrast with LC 518 |
| P3 | LC 712 Minimum ASCII Delete Sum for Two Strings | Medium | Two-sequence DP, fresh |
| P4 | LC 10 Regular Expression Matching | Hard | Two-sequence DP with branching transitions |

**Calibration:** P2 is the sharpest test of the week — it is Coin Change II with the loops swapped, and knowing *why* that changes the answer is exactly Tuesday's lesson. P4 is a famously hard problem; nobody solving it is a fine outcome.

### Reveal (1:45–2:00)
**P2** first: put LC 518 and LC 377 side by side and show that the only difference is which loop is outer, and that this is the combinations-versus-permutations distinction. Then **P1**: mention that it has a one-line answer (`n % 2 == 0`) but that the DP solution is the honest one to find under time pressure — *"in an interview, the DP earns full credit and the trick earns a smile."*

### Assignment 14.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 72 Edit Distance** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 14 (6h) — due Tuesday, Week 15

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 474 Ones and Zeroes | Medium | 45 min |
| LC 673 Number of Longest Increasing Subsequence | Medium | 45 min |
| LC 264 Ugly Number II | Medium | 35 min |
| LC 188 Best Time to Buy and Sell Stock IV | Hard | 55 min |
| LC 115 Distinct Subsequences | Hard | 55 min |

**LC 673** extends Week 13's LIS by carrying a **second** DP array (counts alongside lengths) — a very common interview extension. **LC 188** generalises Thursday's machine to k transactions. **LC 115** has a deceptively simple recurrence and hard-to-get-right base cases.

### Spaced revision (1h) — Weeks 13, 11, 8
| Problem | Source | Target time |
|---|---|---|
| LC 300 Longest Increasing Subsequence | Week 13 (N−1) | under 20 min |
| LC 207 Course Schedule | Week 11 (N−3) | under 20 min |

### Written editorial (1h)
**LC 72 Edit Distance.**

The observation to look for: *"`dp[i][j]` is the minimum operations to turn the first `i` characters of A into the first `j` of B. If the current characters match, no operation is needed and the cost is `dp[i-1][j-1]`. If they differ, the last operation was one of three — replace, delete, or insert — each corresponding to a specific neighbouring cell, so take one plus the cheapest."*

The tell is whether they **name the three transitions and say which cell each corresponds to.** A student who writes "1 + min of three neighbours" without naming them has memorised the formula, not understood it.

---

## Instructor notes

### What usually goes wrong this week
- **The knapsack loop direction is memorised, not understood.** Make them explain *why* forward allows reuse. Ask it in critique and in warm-ups.
- **Students confuse subsequence and substring** at Week 14. Define both again, out loud.
- **Index conventions get mixed** in two-sequence DP. Insist on `(m+1) × (n+1)` with row/column 0 as the empty string, every time.
- **The state machine is copied without being understood.** The "draw your diagram on the board" critique is the fastest check available.
- **Fatigue peaks this week.** Six sessions of DP. Watch attendance and energy; consider cutting a weekend problem for the whole class and saying so — a visible, deliberate reduction is far better for morale than silent non-completion.

### Watch list
This is the last full teaching week before Phase III's integration. **By Friday you should know which students can define a state unaided.** Those who cannot will struggle in Week 16's recognition drill and in Mock Round 3 — plan their Week 15 flex-block time accordingly.

### What to cut if you are behind
1. Thursday's bitmask flex — awareness level, safe to lose
2. LC 583 from Wednesday — LC 1143 and LC 72 carry two-sequence DP
3. LC 115 and LC 264 from the weekend set
4. LC 123 from Thursday's assignment — LC 309 and LC 714 carry the machine

**Never cut:** the loop-direction derivation, the LCS grid drawn by hand, the three named transitions in Edit Distance, or the state machine diagram requirement.
