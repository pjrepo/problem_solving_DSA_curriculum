# Week 13 — DP I · Optional Practice

**Phase II** · Week band: **Medium/Hard** · Reinforce = Easy/Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> DP is the most common reason strong candidates fail interviews, and it is taught here as **memoised recursion** — not as a table-filling ritual. If you are struggling, the fix is almost never more DP problems: it is going back to Week 7 and making sure the recursion is solid, because DP is recursion plus a cache and nothing else.
>
> **Easy DP problems barely exist** — almost every one is in the standard set. Tuesday's Reinforce is genuinely Easy; Wednesday's and Friday's are gentle Mediums, and Thursday's are matrix warm-ups rather than DP, to get the two-index bookkeeping automatic before you add the recurrence on top.

---

## Session 1 (Tuesday) — DP from Recursion

> Optional. Not graded. Skip freely.

### Check your understanding
1. Write the three questions that define any DP problem: what is the state, what is the recurrence, what is the base case. Now answer them for Climbing Stairs.
2. Memoisation and tabulation give the same answer. Name one advantage of each.
3. Your memo key is the function's arguments. Which arguments belong in the key, and how do you tell?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 118 Pascal's Triangle | Easy | 20 min |
| LC 119 Pascal's Triangle II | Easy | 25 min |

Pascal's Triangle is the gentlest possible 2D recurrence — each cell depends on two cells above it. Build it iteratively, then write it recursively with a memo, and compare.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 97 Interleaving String | Medium | 40 min |
| LC 44 Wildcard Matching | **Hard** | 55 min |

LC 97 is the two-pointer state that people find genuinely counter-intuitive: the state is *how much of each string you have consumed*, not the strings themselves. LC 44 is the same idea with wildcards and it is a real Hard.

### 3. Revision — Week 12 (Graphs II & Greedy) · Week 10 (Trees II · Heaps · Tries) · Week 7 (Recursion)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2078 Two Furthest Houses With Different Colors | Easy | 25 min |
| LC 1578 Minimum Time to Make Rope Colorful | Medium | 35 min |

### 4. Interview follow-ups
1. **"What is the state, and why is that sufficient?"** — A good answer names the variables and argues that nothing else about the history affects the future — that is the Markov property that makes DP valid, and stating it explicitly is a strong signal.
2. **"What is the space complexity, and can you reduce it?"** — A good answer spots that if row i depends only on row i−1 you can keep two rows, or sometimes one. Going from O(n²) to O(n) space is the standard follow-up and it is worth having ready.

---

## Session 2 (Wednesday) — The 1D DP Family

> Optional. Not graded. Skip freely.

### Check your understanding
1. House Robber: what is the state, and why is "the index I'm at" not enough on its own for some variants?
2. Decode Ways: what makes the recurrence depend on *two* previous values, and where do the awkward cases live?
3. A 1D DP over n items with O(1) transition is O(n). What input shape would make the transition O(n) and the whole thing O(n²)?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1048 Longest String Chain | Medium | 35 min |
| LC 337 House Robber III | Medium | 40 min |

Gentle **Mediums** — see the note at the top. LC 337 is House Robber on a tree, which is the same recurrence with Week 9's traversal underneath it.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 368 Largest Divisible Subset | Medium | 40 min |
| LC 85 Maximal Rectangle | **Hard** | 55 min |

### 3. Revision — Week 12 (Graphs II & Greedy) · Week 10 (Trees II · Heaps · Tries) · Week 7 (Recursion)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2357 Make Array Zero by Subtracting Equal Amounts | Easy | 25 min |
| LC 654 Maximum Binary Tree | Medium | 35 min |

### 4. Interview follow-ups
1. **"Can you write it bottom-up?"** — A good answer converts the memoised recursion to a loop and says which direction the loop must run so dependencies are ready. Being fluent in both directions is what makes DP feel routine rather than magic.
2. **"Reduce the space to O(1)."** — A good answer identifies exactly how many previous values the recurrence touches and keeps only those. For House Robber that is two variables, and doing it live is a common interview request.

---

## Session 3 (Thursday) — Grid / 2D DP

> Optional. Not graded. Skip freely.

### Check your understanding
1. Grid DP: what does `dp[i][j]` mean in your solution? State it as a sentence, not as code.
2. Which cells does a typical grid recurrence depend on, and what does that tell you about the iteration order?
3. Unique Paths with obstacles — what changes in the recurrence, and what changes in the base row?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2965 Find Missing and Repeated Values | Easy | 20 min |
| LC 2022 Convert 1D Array Into 2D Array | Easy | 25 min |

Neither of these is a DP problem. They are here to make the two-index bookkeeping automatic — the most common grid-DP bug is not the recurrence, it is confusing rows with columns.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1277 Count Square Submatrices with All Ones | Medium | 40 min |
| LC 329 Longest Increasing Path in a Matrix | **Hard** | 55 min |

LC 1277 is a grid recurrence in disguise: the state is "largest square whose bottom-right corner is here". LC 329 needs memoisation over a graph rather than a grid, and it is the bridge between Weeks 11 and 13.

### 3. Revision — Week 12 (Graphs II & Greedy) · Week 10 (Trees II · Heaps · Tries) · Week 7 (Recursion)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2099 Find Subsequence of Length K With the Largest Sum | Easy | 25 min |
| LC 1438 Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why do you iterate in that order?"** — A good answer says the recurrence needs cells above and to the left already computed, so the order is forced. Candidates who cannot justify the loop order usually have not understood the dependency structure.
2. **"Could you do this in O(n) space instead of O(mn)?"** — A good answer keeps a single row and updates it in place, and identifies the one value that must be saved before being overwritten. That detail is the whole difficulty.

---

## Session 4 (Friday) — ARENA: Contest 8 (DP)

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. State, recurrence, base case. From memory, for each of: Climbing Stairs, House Robber, Unique Paths, Coin Change.
2. Which contest problem did you recognise as DP but fail to find the state for? The state is the skill; the recurrence usually follows.
3. Next week is DP II — six more families. What from this week is still shaky?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 907 Sum of Subarray Minimums | Medium | 35 min |
| LC 983 Minimum Cost For Tickets | Medium | 40 min |

Gentle **Mediums**. LC 907 is a monotonic stack problem that many people solve with DP thinking, which makes it a good test of whether you are pattern-matching or reasoning.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 413 Arithmetic Slices | Medium | 40 min |
| LC 312 Burst Balloons | **Hard** | 55 min |

LC 312 is interval DP and the hardest kind of state to see — you reason about the *last* balloon burst, not the first. It is worth reading the editorial even if you do not solve it.

### 3. Revision — Week 12 (Graphs II & Greedy) · Week 10 (Trees II · Heaps · Tries) · Week 7 (Recursion)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1217 Minimum Cost to Move Chips to The Same Position | Easy | 25 min |
| LC 3016 Minimum Number of Pushes to Type Word II | Medium | 35 min |

### 4. Interview follow-ups
1. **"How did you know this was DP and not greedy?"** — A good answer names a case where the locally best choice is wrong, which is exactly what rules greedy out. Being able to produce that counterexample quickly is the discriminator.
2. **"How many distinct states does your solution have, and what does that make the complexity?"** — A good answer multiplies the state space by the transition cost. That single sentence is the correct way to give a DP complexity, and most candidates guess instead.
