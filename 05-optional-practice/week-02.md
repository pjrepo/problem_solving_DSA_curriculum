# Week 2 — Arrays I: Traversal, Prefix Sums, Counting · Optional Practice

**Phase I** · Week band: **Easy → Easy/Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.

---

## Session 1 (Tuesday) — Single-Pass Traversal & Kadane's

> Optional. Not graded. Skip freely.

### Check your understanding
1. Kadane's asks, at every element, "extend the current run or start fresh?" Write the one line that makes that decision, and say what it means when the answer is *start fresh*.
2. Why does Kadane's need only one variable of history, not the whole array?
3. Your running maximum is initialised to `0`. Give an input where that is wrong, and say what it should be instead.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 674 Longest Continuous Increasing Subsequence | Easy | 20 min |
| LC 605 Can Place Flowers | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1749 Maximum Absolute Sum of Any Subarray | Medium | 35 min |
| LC 363 Max Sum of Rectangle No Larger Than K | **Hard** | 60 min |

LC 1749 is Kadane's run twice — once for maximum, once for minimum — and seeing that is the whole problem. LC 363 is Kadane's plus prefix sums plus a sorted structure; it is genuinely out of band and worth reading about even if you do not finish it.

### 3. Revision — Week 1: hash maps, sets, complexity
| Problem | Difficulty | Budget |
|---|---|---|
| LC 205 Isomorphic Strings | Easy | 25 min |
| LC 409 Longest Palindrome | Easy | 25 min |

### 4. Interview follow-ups
1. **"What if I also want the indices of that subarray, not just the sum?"** — A good answer keeps a candidate start pointer that resets on the same condition the sum does. Say that the complexity does not change; only the bookkeeping does.
2. **"What if the array is all negative?"** — A good answer has already handled it via the initialisation. This is the single most common Kadane's bug and interviewers know to probe it.

---

## Session 2 (Wednesday) — Prefix Sums & Difference Arrays

> Optional. Not graded. Skip freely.

### Check your understanding
1. `prefix[i]` — does it include `arr[i]` or stop before it? Both conventions work; say which you use and what that makes the range-sum formula.
2. Why is the prefix array conventionally one element longer than the input?
3. A difference array makes range *updates* O(1) but range *reads* expensive. Explain the trade in one sentence, and say which problem shape wants it.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2574 Left and Right Sum Differences | Easy | 20 min |
| LC 1652 Defuse the Bomb | Easy | 30 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 304 Range Sum Query 2D - Immutable | Medium | 40 min |
| LC 732 My Calendar III | **Hard** | 55 min |

LC 304 is today's idea with one more index — derive the four-term formula rather than looking it up. LC 732 is the difference array in its natural habitat: a sweep over start and end events.

### 3. Revision — Week 1: hash maps and sets
| Problem | Difficulty | Budget |
|---|---|---|
| LC 594 Longest Harmonious Subsequence | Easy | 25 min |
| LC 532 K-diff Pairs in an Array | Medium | 30 min |

### 4. Interview follow-ups
1. **"The array changes between queries. Does your prefix sum still work?"** — A good answer says no, and names the cost: a single update invalidates O(n) of the prefix array. Then it says what you would reach for instead if updates were frequent — and admits that structure is out of scope if you have not learned it.
2. **"How much extra memory does this use, and could you avoid it?"** — A good answer notes you can sometimes prefix in place if the caller does not need the original, and asks whether that is allowed rather than assuming.

---

## Session 3 (Thursday) — Hash-Map Counting Patterns

> Optional. Not graded. Skip freely.

### Check your understanding
1. Grouping anagrams needs a *key* that is identical for all members of a group. Name two valid keys and say which is cheaper.
2. `Counter(a) == Counter(b)` versus `sorted(a) == sorted(b)` — both decide anagrams. Give the complexity of each and say which you would offer in an interview.
3. You are counting pairs whose sum is divisible by 60. Why is the count of *remainders* the useful thing to store, not the count of values?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 819 Most Common Word | Easy | 25 min |
| LC 748 Shortest Completing Word | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 791 Custom Sort String | Medium | 30 min |
| LC 895 Maximum Frequency Stack | **Hard** | 55 min |

LC 895 is counting plus a dictionary *of stacks*. It is a design problem and a genuine preview of Week 15 — the insight is what to key on.

### 3. Revision — Week 1: sets and counting
| Problem | Difficulty | Budget |
|---|---|---|
| LC 575 Distribute Candies | Easy | 20 min |
| LC 1160 Find Words That Can Be Formed by Characters | Easy | 25 min |

### 4. Interview follow-ups
1. **"Your key is a sorted string. What is the complexity, counting the key construction?"** — A good answer gives O(n · k log k) for n words of length k, and notes that a 26-slot count tuple makes it O(n · k). Most candidates quote O(n) and get caught here.
2. **"Could you do this in one pass instead of two?"** — A good answer says whether one pass is even possible for this problem before claiming it. Some counting problems genuinely need the full counts before deciding anything; saying so is a better answer than a wrong optimisation.

---

## Session 4 (Friday) — ARENA: Contest 1

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. For each contest problem you did not solve: name the pattern in three words. If you cannot, that is the thing to go and read.
2. Which problem did you spend longest on, and at what minute should you have moved on?
3. Where did you place on the leaderboard versus the diagnostic contest? What is one specific thing that changed?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 628 Maximum Product of Three Numbers | Easy | 25 min |
| LC 561 Array Partition | Easy | 20 min |

Both are "sort, then scan" — the cheapest pattern in the course and one that wins contest points regularly.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 665 Non-decreasing Array | Medium | 35 min |
| LC 220 Contains Duplicate III | **Hard** | 55 min |

LC 220 is the direct sequel to Thursday's LC 219 and it is much harder than it looks. It also previews the bucketing idea you will meet again in Week 8.

### 3. Revision — the whole of Week 1
| Problem | Difficulty | Budget |
|---|---|---|
| LC 961 N-Repeated Element in Size 2N Array | Easy | 15 min |
| LC 697 Degree of an Array | Easy | 30 min |

### 4. Interview follow-ups
1. **"Talk me through the problem you ran out of time on."** — A good answer separates *did not know the pattern* from *knew it and was slow*. Those need completely different fixes, and only you can tell which it was.
2. **"You sorted the input. Was that necessary?"** — A good answer states what sorting bought (order, adjacency of equal elements) and what it cost (O(n log n), and destroying the original indices). If the problem needed original indices, sorting was a bug, not an optimisation.
