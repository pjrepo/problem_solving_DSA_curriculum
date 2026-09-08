# Week 13 — Dynamic Programming I: Memoization, 1D & Grid

**Phase II: Techniques** · Difficulty band: **Medium/Hard**

> DP is the most common reason otherwise-strong candidates fail interviews, and it is taught here as **six named families across two weeks** rather than as one intimidating topic. The approach throughout: **always start from memoized recursion, never from a table.** Students already know recursion (Week 7) and they already know memoization (Week 7, Wednesday). DP is those two things plus one new discipline — **defining the state precisely.**
>
> Week 12 ended with a greedy algorithm that failed on coin change. Week 13 fixes it.

---

## Exit criteria

- [ ] Recognise DP triggers: "how many ways", "min/max cost", overlapping subproblems
- [ ] State what `dp[i]` **means** in one English sentence before writing any code
- [ ] Convert a brute-force recursion into a memoized one, then into a table
- [ ] Explain the difference between top-down memoization and bottom-up tabulation
- [ ] Write 1D DP for the climbing/robbing, coin change, LIS and word-break families
- [ ] Write 2D grid DP and optimise it to a single row
- [ ] Say why greedy fails on coin change and DP does not

---

## Session 1 (Tuesday) — DP from Recursion

### Weekend-set debrief (0:00–0:15)
Debrief **LC 134 Gas Station** — the correctness argument, not the code. This was the term's clearest test of whether students can *justify* rather than *describe*; report honestly on what you saw across the 14 editorials.

### Concept spine (0:15–0:50)

**Part A — resolve Thursday's cliffhanger, immediately.**

Put it back on the board: coins `[1, 3, 4]`, target 6. Greedy gives 3 coins; optimal is 2.

*"Greedy failed because taking the 4 foreclosed a better option. So let's not choose — let's try every option and keep the best."*

```python
def coin_change(coins, amount):
    def best(rem):
        if rem == 0: return 0
        if rem < 0:  return float('inf')
        return 1 + min(best(rem - c) for c in coins)
    return best(amount)
```

*"Correct. What's the complexity?"* → exponential. *"And what is it doing wrong?"* → recomputing `best(2)` many times.

> **"You already know this shape. It is `fib` from Week 7."**

Add three lines and it is O(amount × coins):

```python
def coin_change(coins, amount):
    memo = {}
    def best(rem):
        if rem == 0: return 0
        if rem < 0:  return float('inf')
        if rem in memo: return memo[rem]
        memo[rem] = 1 + min(best(rem - c) for c in coins)
        return memo[rem]
    ...
```

> ***"That is dynamic programming. You wrote your first one in Week 7. Everything from here is practice at one specific skill: defining the state."***

**Part B — the five-step procedure.** Put it on the board and leave it there for two weeks.

1. **Define the state.** *What does `dp[i]` mean, in one English sentence?*
2. **Write the recurrence.** How does `dp[i]` follow from smaller states?
3. **Base cases.**
4. **Order of computation.** Which states must exist before which?
5. **Optimise space** — only after the rest works.

> **Step 1 is where every failure happens.** If you cannot finish the sentence *"`dp[i]` is the ..."*, you cannot write the recurrence, and no amount of staring at the code will help.

**Part C — climbing stairs, all four versions.** Do all four on the board; the progression is the lesson.

```python
# 1. Brute force — O(2^n)
def climb(n):
    if n <= 2: return n
    return climb(n-1) + climb(n-2)

# 2. Memoized (top-down) — O(n) time, O(n) space + O(n) stack
def climb(n, memo={}):
    if n <= 2: return n
    if n not in memo: memo[n] = climb(n-1, memo) + climb(n-2, memo)
    return memo[n]

# 3. Tabulated (bottom-up) — O(n) time, O(n) space, no stack
def climb(n):
    dp = [0] * (n + 1)                 # dp[i] = number of ways to reach step i
    dp[1], dp[2] = 1, 2
    for i in range(3, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]

# 4. Space-optimised — O(n) time, O(1) space
def climb(n):
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```

**Top-down versus bottom-up — say this plainly:**

| | Top-down (memo) | Bottom-up (table) |
|---|---|---|
| Written as | recursion + cache | loops |
| Computes | only the states you need | every state |
| Risk | stack overflow on deep recursion | none |
| Easier to write | **usually yes** | after practice |
| Space optimisation | harder | **easy** |

> ***"In an interview, write the memoized version first. It follows directly from the brute force, so it is much harder to get wrong under pressure. Convert to a table only if asked, or if you need the space optimisation."*** This is practical advice students will actually use.

**Part D — House Robber, where the state needs thought.**

*"Rob houses along a street, but never two adjacent. Maximise the total."*

Ask for the state and **wait**. Wrong answers to expect: "`dp[i]` is whether I rob house i" (not a value), "`dp[i]` is the money from house i" (already given).

> **`dp[i]` = the maximum money obtainable from the first `i` houses.**

Then the recurrence writes itself: at house `i` you either skip it (`dp[i-1]`) or rob it (`dp[i-2] + nums[i]`).

**Make the point explicitly:** *"The recurrence was easy the moment the state was right. That is why step 1 gets all the attention."*

### Live-code (0:50–1:10)
Climbing Stairs through all four versions, then **LC 198 House Robber** — state sentence first, out loud, before any code.

### Guided practice (1:15–1:50)
1. **LC 70 Climbing Stairs** — Easy
2. **LC 746 Min Cost Climbing Stairs** — Easy. Same shape, different objective.
3. **LC 198 House Robber** — Medium
4. **LC 213 House Robber II** — Medium. Circular street; the trick is running the linear version twice (excluding the first house, then the last).

**Require a written state sentence in a comment above every solution this week.** No sentence, no marks.

### Live critique (1:50–2:00)
An *LC 198*. Focus: **read the author's state sentence aloud.** If it is vague, the recurrence usually has a subtle bug. This is the diagnostic to repeat all week.

### Flex (2:00–2:30)
**LC 152 Maximum Product Subarray**, re-solved as explicit DP. They met it in Week 2 as a Kadane variant. Now name it: you carry **two** states (running max and running min) because a negative flips the ordering. *"Sometimes the state is more than one number — that is the whole difficulty."*

### Common misconceptions
- Starting with a table before knowing what a cell means.
- A state that is not a value (a boolean "did I take it") when a value is needed.
- Off-by-one between `dp[i]` meaning "first i items" versus "up to index i". **Pick one convention and state it.**
- Mutable default `memo={}` shared across calls.
- Believing DP is a distinct technique from recursion rather than recursion plus caching.

### Assignment 13.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 70 Climbing Stairs | Easy | 20 min |
| LC 746 Min Cost Climbing Stairs | Easy | 25 min |
| LC 198 House Robber | Medium | 30 min |
| LC 213 House Robber II | Medium | 35 min |

**Every solution must carry its state sentence in a comment.** Graded.

---

## Session 2 (Wednesday) — The 1D DP Family

### Warm-up (0:00–0:10)
1. State the five steps.
2. What does `dp[i]` mean in House Robber?
3. Memoization or tabulation in an interview, and why?

### Concept spine (0:10–0:45)

**Part A — the family, named.** All of these share the shape *"the answer at `i` depends on a few earlier answers."*

| Problem | `dp[i]` means | Recurrence |
|---|---|---|
| Climbing Stairs | ways to reach step i | `dp[i-1] + dp[i-2]` |
| House Robber | max money from first i houses | `max(dp[i-1], dp[i-2] + nums[i])` |
| Coin Change | fewest coins to make amount i | `1 + min(dp[i-c] for c in coins)` |
| Word Break | can `s[:i]` be segmented? | `any(dp[j] and s[j:i] in words)` |
| Decode Ways | ways to decode `s[:i]` | `dp[i-1]·(valid 1-digit) + dp[i-2]·(valid 2-digit)` |

*"Five problems, one shape. What changes is the state's meaning and what counts as a valid previous step."*

**Part B — LIS, where the state surprises people.** LC 300, Longest Increasing Subsequence.

The natural-sounding state — *"`dp[i]` = the LIS of the first i elements"* — **does not work**, because you cannot extend it: you do not know what it ended with. Let students hit this.

> **`dp[i]` = the length of the longest increasing subsequence that ENDS at index `i`.**

Now `dp[i] = 1 + max(dp[j] for j < i if nums[j] < nums[i])`, and the answer is `max(dp)`, not `dp[-1]`.

**Two lessons worth naming:**
1. *"Ending at i"* is a very common state formulation — it makes the state extendable
2. **The answer is not always the last cell.** Watch for this.

O(n²). Mention the O(n log n) patience-sorting version exists and show it in the flex block.

**Part C — Word Break, and the subtlety.** `dp[i]` = *"can the first `i` characters be segmented?"* The recurrence tries every split point `j`: if `dp[j]` is true and `s[j:i]` is a word, then `dp[i]` is true. **Use a set for the dictionary** — an O(n) list lookup inside a double loop is a hidden O(n³).

### Live-code (0:45–1:10)
**LC 322 Coin Change** both top-down and bottom-up, explicitly closing Week 12's cliffhanger. Then **LC 300 LIS**, after letting the room propose the wrong state first.

### Guided practice (1:15–1:50)
1. **LC 322 Coin Change** — Medium
2. **LC 139 Word Break** — Medium
3. **LC 300 Longest Increasing Subsequence** — Medium
4. **LC 91 Decode Ways** — Medium. The edge cases (leading zeros, `"06"`) are most of the work.

### Live critique (1:50–2:00)
An *LC 300*. Focus: is the state "ending at i"? And did they return `max(dp)` rather than `dp[-1]`? Both errors are common and both are conceptual, not typographical.

### Flex (2:00–2:30)
**LC 300 in O(n log n)** — maintain an array of the smallest possible tail for each length and binary search it. *"It is Week 8's `lower_bound`."* Connecting binary search to DP is genuinely delightful, and the pairing is worth the time.

### Common misconceptions
- LIS state as "first i elements" instead of "ending at i".
- Returning `dp[-1]` where `max(dp)` is required.
- Word Break with a list instead of a set.
- Coin Change confusing "no solution" (infinity) with 0.
- Decode Ways mishandling `'0'`.

### Assignment 13.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 322 Coin Change | Medium | 30 min |
| LC 279 Perfect Squares | Medium | 25 min |
| LC 139 Word Break | Medium | 30 min |
| LC 91 Decode Ways | Medium | 35 min |

LC 279 is Coin Change with the coins being perfect squares — assigned immediately after so the generalisation is unmissable.

---

## Session 3 (Thursday) — Grid / 2D DP

### Warm-up (0:00–0:10)
1. What is the LIS state, and why not "first i elements"?
2. Why does Word Break need a set?
3. Coin Change: what does `dp[amount]` mean?

### Concept spine (0:10–0:45)

**Part A — two dimensions, same procedure.**

**LC 62 Unique Paths.** Move only right or down from the top-left to the bottom-right of an m×n grid. How many paths?

State: **`dp[r][c]` = the number of paths from the start to cell `(r, c)`.**

Recurrence: `dp[r][c] = dp[r-1][c] + dp[r][c-1]` — you arrived from above or from the left.

Base: the entire first row and first column are 1 (only one way along an edge).

**Order matters:** fill row by row, left to right, so both contributing cells already exist. **Make "which cells must exist before this one?" an explicit question** for every 2D DP — it is step 4 of the procedure and students routinely skip it.

**Part B — space optimisation, derived.**

*"When computing row r, which rows do you actually reference?"* → only `r-1`.

> **So you only need one row.** Keep a single array and update it in place: `dp[c] += dp[c-1]`, where `dp[c]` still holds the row above and `dp[c-1]` already holds the current row.

O(m·n) space → **O(n)**. Trace it once on the board — it looks like a trick until you see the two roles of the array.

*"This is a standard interview follow-up: 'can you reduce the space?' For most grid DP the answer is 'keep one row'."*

**Part C — variations.**
- **LC 63 Unique Paths II** — obstacles: a blocked cell is simply 0 ways
- **LC 64 Minimum Path Sum** — `min` instead of `+`, plus the cell's own cost
- **LC 120 Triangle** — a non-rectangular grid; **going bottom-up is much cleaner than top-down**, which is worth noticing

*"Same skeleton, four problems. Once the state is right, the variations are small."*

### Live-code (0:45–1:10)
**LC 62** as a full 2D table, then the one-row optimisation. Then **LC 64 Minimum Path Sum**.

### Guided practice (1:15–1:50)
1. **LC 62 Unique Paths** — Medium
2. **LC 63 Unique Paths II** — Medium
3. **LC 64 Minimum Path Sum** — Medium
4. **LC 120 Triangle** — Medium

### Live critique (1:50–2:00)
An *LC 64*. Focus: how were the first row and column initialised? Boundary initialisation is where most grid DP bugs live, and doing it inside the main loop with conditionals is usually cleaner than a separate pre-pass — worth discussing both.

### Flex (2:00–2:30)
**LC 221 Maximal Square** — Medium. The state is unobvious: `dp[r][c]` = the side length of the largest square whose **bottom-right corner** is `(r, c)`. Recurrence: `1 + min` of the three neighbours. A satisfying "the state is the whole problem" example.

### Common misconceptions
- Wrong fill order, so a cell reads an uncomputed neighbour.
- Boundary rows and columns initialised incorrectly.
- Space-optimising before the plain version works.
- In LC 221, taking `max` of the three neighbours instead of `min`.

### Assignment 13.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 62 Unique Paths | Medium | 25 min |
| LC 63 Unique Paths II | Medium | 25 min |
| LC 64 Minimum Path Sum | Medium | 30 min |
| LC 120 Triangle | Medium | 35 min |

---

## Session 4 (Friday) — ARENA: Contest 8 (DP)

**Mode:** Contest · 90 minutes · **calibrate this one generously — DP fatigue is real**

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 1137 N-th Tribonacci Number | Easy | Linear DP, minimal |
| P2 | LC 740 Delete and Earn | Medium | **House Robber in disguise** |
| P3 | LC 931 Minimum Falling Path Sum | Medium | Grid DP, fresh |
| P4 | LC 174 Dungeon Game | Hard | Grid DP that must be filled **backwards** |

**Calibration:** P2 is the transfer test of the week — once you bucket the values by count, it is exactly House Robber. P4 is genuinely hard: filling forward does not work because the constraint applies at the *end*, and recognising that is the whole problem.

### Reveal (1:45–2:00)
**P2** first: show the reduction to House Robber explicitly. Then **P4**: *"You tried to fill from the start. But the requirement 'HP must stay above 0 the whole way' depends on the future, not the past — so define `dp[r][c]` as the minimum HP needed **on entering** this cell to survive from here to the end, and fill backwards from the exit."*

That is a genuinely deep lesson: **the direction of the DP is a choice, driven by where the constraint lives.**

### Assignment 13.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 322 Coin Change** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 13 (6h) — due Tuesday, Week 14

Five problems rather than six. DP is dense and rushed practice teaches nothing.

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 221 Maximal Square | Medium | 45 min |
| LC 96 Unique Binary Search Trees | Medium | 45 min |
| LC 343 Integer Break | Medium | 40 min |
| LC 918 Maximum Sum Circular Subarray | Medium | 45 min |
| LC 1043 Partition Array for Maximum Sum | Medium | 50 min |

**LC 96** is a lovely state: `dp[n]` = the number of BSTs with n nodes, summed over every choice of root. **LC 918** is Kadane from Week 2 with a circular twist (the answer is either a normal maximum subarray, or the total minus the *minimum* subarray) — a nice callback. **LC 1043** requires a state over partition endpoints and is the hardest here.

### Spaced revision (1h) — Weeks 12, 10, 7
| Problem | Source | Target time |
|---|---|---|
| LC 55 Jump Game | Week 12 (N−1) | under 15 min |
| LC 486 Predict the Winner | Week 7 (N−6) | under 30 min |

**Both chosen deliberately.** LC 55 is a *greedy* problem sitting in a DP week — direct interference training, since it also has a valid DP solution and students should be able to say why greedy is the better answer here. LC 486 is the recursion whose state-definition discipline this whole week is built on; re-solving it now should feel completely different than it did in Week 7.

### Written editorial (1h)
**LC 322 Coin Change.**

The observation to look for: *"Greedy fails because a locally largest coin can foreclose a better combination. So define `dp[a]` as the fewest coins making amount `a`, and treat every coin as a candidate for the **last** coin used — `dp[a] = 1 + min(dp[a - c])` over all coins that fit. Every option stays open, which is exactly what greedy could not do."*

The phrase to look for is **"candidate for the last coin."** That framing — *what was the final move?* — generalises to most DP problems, and students who find it will handle Week 14 far better.

---

## Instructor notes

### What usually goes wrong this week
- **Students want the table first.** Resist it. Brute force → memo → table, every single time. The table is an optimisation, not the idea.
- **State sentences are vague** ("dp[i] is the answer at i"). Push until they are precise. This is the whole skill.
- **The LIS state trap catches nearly everyone.** Let it. Do not pre-warn.
- **DP fatigue is real and starts around Thursday.** Weeks 13–14 are six sessions of the hardest topic. Watch morale; the contest is deliberately generous and the weekend set is one problem short.
- **Students who struggled in Week 7 will struggle badly here.** DP is memoized recursion. Cross-reference your Week 7 paper-tracing notes and Week 12's greedy-versus-DP drill.

### Watch list
The state sentence is your diagnostic. A student who writes precise state sentences will be fine in Week 14. A student writing vague ones is pattern-matching, and Week 14's string DP will expose it immediately. **Read every state sentence in this week's submissions** — it is faster than reading the code and far more informative.

### What to cut if you are behind
1. Wednesday's flex (LIS in O(n log n)) — lovely but not essential
2. LC 213 from Tuesday
3. LC 1043 and LC 343 from the weekend set
4. LC 120 from Thursday — LC 62/63/64 carry grid DP

**Never cut:** the coin change cliffhanger resolution, the five-step procedure, all four versions of Climbing Stairs, the written state sentence requirement, or the one-row space optimisation.
