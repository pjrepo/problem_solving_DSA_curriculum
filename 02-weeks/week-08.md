# Week 8 — Binary Search & Sorting as a Tool

**Phase II: Techniques** · Difficulty band: **Medium**

> Binary search is where "I know the idea" and "I can write it correctly" diverge most sharply. Almost every student can describe it; a minority can write one that terminates correctly on every input. The cure is **one template, used every time, with a stated invariant** — never improvised boundaries.
>
> Wednesday's session, **binary search on the answer space**, is the highest-value single idea in Phase II. Most self-taught candidates never learn it, and it converts a family of Hard-looking problems into routine ones.

**Before teaching:** check your Week 7 Friday decision. If fewer than 9 students solved Contest 4's P1 and P2, spend this week's flex blocks on recursion remediation.

---

## Exit criteria

- [ ] Write binary search from memory with a stated loop invariant, correct on empty input and on a missing target
- [ ] Explain why `mid = lo + (hi - lo) // 2` is preferred, and why the loop terminates
- [ ] Implement lower bound and upper bound, and say what each returns
- [ ] Recognise "minimum X such that…" as binary search on the answer space
- [ ] Write a monotonic feasibility check and justify its monotonicity
- [ ] Choose a sort key (start vs end) for interval problems and justify the choice
- [ ] Write a custom comparator

---

## Session 1 (Tuesday) — Binary Search on Arrays

### Weekend-set debrief (0:00–0:15)
Debrief **LC 486 Predict the Winner** — the score-difference reframing. **Read all 14 editorials before this session**; they are your best predictor of who will struggle in Week 13. Name the students in your tracker.

Then briefly report the Contest 4 recursion outcome to the room, honestly and without singling anyone out.

### Concept spine (0:15–0:45)

**Part A — one template, forever.**

Ask who has written a binary search that infinite-looped. Most hands. *"That is not carelessness. It is what happens without an invariant."*

```python
def binary_search(a, target):
    lo, hi = 0, len(a) - 1          # INVARIANT: if target exists, it is in a[lo..hi]
    while lo <= hi:
        mid = lo + (hi - lo) // 2   # avoids overflow in fixed-width languages
        if a[mid] == target:
            return mid
        elif a[mid] < target:
            lo = mid + 1            # a[mid] ruled out
        else:
            hi = mid - 1            # a[mid] ruled out
    return -1
```

**Four things to justify explicitly, out loud:**

1. **The invariant.** *"If the target is anywhere, it is in `a[lo..hi]`."* Every line preserves this.
2. **`lo <= hi`, not `lo < hi`.** With `<`, a single-element range is never examined.
3. **`mid + 1` and `mid - 1`, not `mid`.** Assigning `lo = mid` when `lo == mid` never shrinks the range → infinite loop. **This is the bug.**
4. **The range shrinks every iteration**, so it terminates.

> *"If you can state the invariant, you will write it correctly. If you are guessing at `<` versus `<=`, you will not."*

**Part B — lower and upper bound.** More useful in practice than plain search, and the basis of LC 34.

- **lower_bound(target)** — index of the first element **≥** target
- **upper_bound(target)** — index of the first element **>** target

Teach **lower bound only**, and derive upper bound from it (`lower_bound(target + 1)` for integers). Two templates is one too many.

```python
def lower_bound(a, target):
    lo, hi = 0, len(a)              # note: hi = len(a), a half-open range
    while lo < hi:                  # note: strict <
        mid = (lo + hi) // 2
        if a[mid] < target: lo = mid + 1
        else:                hi = mid
    return lo                       # may be len(a) if all elements are smaller
```

**Point at the differences deliberately:** `hi = len(a)` and `lo < hi` and `hi = mid`. This is a *different* invariant — "the answer is in `[lo, hi)`" — and it is why the boundaries differ. Understanding that the boundaries follow from the invariant is the entire lesson.

Note that Python's `bisect_left` / `bisect_right` are exactly these, and are fine to use once students can write them.

**Part C — beyond sorted arrays.** Binary search works whenever there is a **monotonic predicate** — some property that is false, false, false, then true, true, true. Sortedness is one instance of this, not the requirement.

*"Hold that thought. Tomorrow it becomes the whole session."*

### Live-code (0:45–1:05)
`binary_search`, then `lower_bound`, then **LC 34 Find First and Last Position** built from two lower-bound calls. Show that the "hard" problem is two calls to a function they already have.

### Guided practice (1:10–1:50)
1. **LC 704 Binary Search** — Easy. The template, verbatim.
2. **LC 35 Search Insert Position** — Easy. This *is* lower bound.
3. **LC 278 First Bad Version** — Easy. The first monotonic-predicate problem: `isBad` is false then true.
4. **LC 34 Find First and Last Position of Element in Sorted Array** — Medium

### Live critique (1:50–2:00)
An *LC 34*. Focus: two clean lower-bound calls, or one search plus linear scanning outward? The latter is O(n) in the worst case (all elements equal) — a correct-looking solution that fails the complexity requirement, which is a valuable thing to see.

### Flex (2:00–2:30)
**LC 33 Search in Rotated Sorted Array** — Medium. At each step one half is guaranteed sorted; determine which, then decide whether the target lies inside it. The classic, and a good test of invariant discipline.

### Common misconceptions
- `lo = mid` instead of `mid + 1` → infinite loop.
- Mixing the two templates' boundary conventions.
- Assuming the array is sorted without checking the problem statement.
- Forgetting the empty-array case.
- Believing binary search requires a sorted array (it requires a monotonic predicate).

### Assignment 8.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 704 Binary Search | Easy | 15 min |
| LC 35 Search Insert Position | Easy | 15 min |
| LC 278 First Bad Version | Easy | 20 min |
| LC 34 Find First and Last Position | Medium | 35 min |
| LC 153 Find Minimum in Rotated Sorted Array | Medium | 35 min |

---

## Session 2 (Wednesday) — Binary Search on the Answer Space

> **The highest-value session of Phase II.** Most candidates never learn this, and it turns a family of Hard-looking problems into mechanical ones.

### Warm-up (0:00–0:10)
1. State the binary search invariant.
2. Why `mid + 1` and not `mid`?
3. What does lower bound return when every element is smaller than the target?

### Concept spine (0:10–0:45)

**Start with a problem that looks nothing like binary search.**

> **LC 875 Koko Eating Bananas.** Piles of bananas, h hours. Koko picks an eating speed k (bananas/hour) and eats from one pile per hour. Find the **minimum k** that finishes all piles within h hours.

Let students flounder for a couple of minutes. Someone will suggest trying every speed from 1 upward.

*"Good — that works. What is the complexity?"* → O(max_pile × n). Too slow.

**Now the reframe:**

> *"We are not searching the array. We are searching the **range of possible answers** — every speed from 1 to max(piles). And here is the key property: if speed k works, then every speed faster than k also works. False, false, false, true, true, true. **That is monotonic, so we can binary search it.**"*

Draw the boolean line on the board:

```
speed:     1     2     3     4     5     6     7     8
works?     F     F     F     F     T     T     T     T
                                   ↑
                        we want this boundary
```

**The recipe — put it on the board and leave it there:**

1. **What is the answer's range?** `[lo, hi]`
2. **Write `feasible(x)`** — can we achieve the goal with x? Usually a simple O(n) greedy scan.
3. **Verify monotonicity.** If x works, does x+1 always work? *(If not, this technique does not apply — say so explicitly.)*
4. **Binary search for the boundary.**

```python
def min_eating_speed(piles, h):
    def feasible(k):
        return sum((p + k - 1) // k for p in piles) <= h   # ceiling division

    lo, hi = 1, max(piles)
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid): hi = mid          # mid might be the answer — keep it
        else:             lo = mid + 1      # mid is too slow — discard it
    return lo
```

O(n · log(max_pile)).

**The two diagnostic questions**, which students should ask of every "minimum/maximum X such that" problem:
1. *If I guess X, can I check feasibility quickly?*
2. *Is feasibility monotonic in X?*

**Yes to both → binary search the answer.** Make them recite this.

### Live-code (0:45–1:05)
LC 875 in full, then **LC 1011 Capacity To Ship Packages Within D Days** — a different story, an identical structure. Write them side by side so the shared skeleton is unmistakable. The point is that the *story* varies and the *technique* does not.

### Guided practice (1:10–1:50)
1. **LC 875 Koko Eating Bananas** — Medium
2. **LC 1011 Capacity To Ship Packages Within D Days** — Medium
3. **LC 1482 Minimum Number of Days to Make m Bouquets** — Medium. The feasibility check is the interesting part.

Three problems only; each needs a properly reasoned feasibility function.

### Live critique (1:50–2:00)
An *LC 1482*. Focus on `feasible()`. Is it correct, is it O(n), and — the question to actually ask — **can the author justify that it is monotonic?** Most will not have thought about it. That is the habit being built.

### Flex (2:00–2:30)
**LC 410 Split Array Largest Sum** — Hard by reputation, routine with this technique. Binary search the largest allowed subarray sum; feasibility is a greedy count of how many pieces you need. Demonstrating that a Hard collapses into the recipe is the best possible advertisement for it.

### Common misconceptions
- Setting `lo`/`hi` to array indices instead of the answer's value range.
- A feasibility check that is not monotonic — technique silently gives wrong answers.
- `hi = mid - 1` when `mid` might itself be the answer.
- Integer division for ceilings: `(p + k - 1) // k`, not `p // k`.

### Assignment 8.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 875 Koko Eating Bananas | Medium | 30 min |
| LC 1011 Capacity To Ship Packages | Medium | 30 min |
| LC 1482 Minimum Days to Make m Bouquets | Medium | 30 min |
| LC 410 Split Array Largest Sum | Hard | 45 min |

---

## Session 3 (Thursday) — Sorting as a Tool & Intervals

### Warm-up (0:00–0:10)
1. What two questions decide whether you can binary search the answer?
2. In LC 875, what is the search space?
3. What would break if `feasible` were not monotonic?

### Concept spine (0:10–0:45)

**Part A — sorting is a preprocessing step, not a topic.**

*"You will almost never be asked to implement a sort. You will constantly be asked to notice that sorting first makes the problem easy."*

The recurring shape: **sort, then a single linear scan.** Cost: O(n log n) to buy an O(n) scan — almost always worth it.

**Custom comparators.** In Python, `key=` covers most needs:
```python
people.sort(key=lambda p: (p.age, -p.height))    # age ascending, height descending
```
For genuinely relational orders (LC 179 from Week 4: is `a+b > b+a`?), use `functools.cmp_to_key`. **In JavaScript, `sort()` compares as strings by default** — `arr.sort((a,b) => a-b)`. This still catches people out in Week 8.

**Part B — intervals, and the sort key as the decision.**

> **The whole difficulty of interval problems is choosing what to sort by.**

| Goal | Sort by | Why |
|---|---|---|
| **Merge** overlapping intervals | **start** | Process left to right; overlap means `start ≤ current_end` |
| **Maximum non-overlapping** count | **end** | Greedy: finishing earliest leaves the most room |
| **Minimum removals** to remove overlaps | **end** | Same greedy, complemented |
| Point coverage / room counts | either, then sweep | Sweep line, or a difference array |

Derive **LC 56 Merge Intervals** with them: sort by start, then scan — if the current interval starts before the previous one ended, merge by extending the end; otherwise, start a new interval.

Then **LC 435 Non-overlapping Intervals**, and ask why sorting by *end* is right. The exchange argument: *"among all intervals that could come next, the one finishing earliest leaves at least as much room as any other, so choosing it is never worse."* First real exposure to greedy justification — Week 12 formalises it.

**Part C — the sweep line.** For "how many are active at time t", sort the events (+1 at start, −1 at end) and sweep. **This is Week 2's difference array in another costume** — say so.

### Live-code (0:45–1:05)
**LC 56 Merge Intervals**, then **LC 435 Non-overlapping Intervals**, deliberately contrasting the sort keys.

### Guided practice (1:10–1:50)
1. **LC 56 Merge Intervals** — Medium
2. **LC 57 Insert Interval** — Medium. Three phases: before, overlapping, after.
3. **LC 435 Non-overlapping Intervals** — Medium
4. **LC 452 Minimum Number of Arrows to Burst Balloons** — Medium. LC 435 with the inequality changed; a good test of whether the greedy was understood.

### Live critique (1:50–2:00)
An *LC 435*. Focus: sorted by end or by start? If by start, does it still work — and can the author say why not? Watching a plausible-looking greedy fail on a counterexample is the best possible preparation for Week 12.

### Flex (2:00–2:30)
**LC 1094 Car Pooling** — Medium. Solvable by sweep line or difference array. Solve it both ways and connect explicitly back to Week 2.

*(Note: LC 252 / 253 "Meeting Rooms" are the classic problems here but are LeetCode Premium. LC 1094 is the free equivalent and is used throughout this curriculum instead.)*

### Common misconceptions
- Sorting by start when the greedy needs end.
- Merging with `<` where `<=` is needed — do `[1,4]` and `[4,5]` overlap? Ask the interviewer; this is a legitimate clarifying question.
- Mutating a list while iterating it.
- Forgetting that sorting is O(n log n) and dominates a subsequent O(n) scan.

### Assignment 8.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 56 Merge Intervals | Medium | 30 min |
| LC 57 Insert Interval | Medium | 35 min |
| LC 435 Non-overlapping Intervals | Medium | 30 min |
| LC 452 Minimum Number of Arrows | Medium | 25 min |

---

## Session 4 (Friday) — ARENA: Contest 5

**Mode:** Contest · 90 minutes

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 744 Find Smallest Letter Greater Than Target | Easy | Upper bound, fresh |
| P2 | LC 162 Find Peak Element | Medium | Binary search **without** a sorted array |
| P3 | LC 1552 Magnetic Force Between Two Balls | Medium | Binary search the answer, never taught |
| P4 | LC 4 Median of Two Sorted Arrays | Hard | Partition-based binary search |

**Calibration:** P2 is the interesting one — it tests whether students internalised "monotonic predicate" rather than "sorted array". P3 is a pure transfer test of Wednesday's recipe on an unseen story.

### Reveal (1:45–2:00)
Spend the time on **P2 and P3**. For P2: *"The array isn't sorted, and it still works, because 'the neighbour is larger' is a monotonic predicate for this problem."* For P3: walk the recipe — range, feasibility, monotonicity, search — and show it is the same four steps as Koko.

### Assignment 8.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 4 Median of Two Sorted Arrays** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 8 (6h) — due Tuesday, Week 9

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 74 Search a 2D Matrix | Medium | 30 min |
| LC 240 Search a 2D Matrix II | Medium | 40 min |
| LC 81 Search in Rotated Sorted Array II | Medium | 40 min |
| LC 646 Maximum Length of Pair Chain | Medium | 35 min |
| LC 1094 Car Pooling | Medium | 40 min |
| LC 2064 Minimized Maximum of Products Distributed to Any Store | Medium | 40 min |

**LC 74 vs LC 240 is the pairing to notice:** 74 is a true binary search on a flattened sorted array; 240 is *not* — it needs a staircase walk from the top-right. Students who apply binary search to 240 will get it wrong, and that is the lesson. **LC 81** shows how duplicates break the rotated-array guarantee and degrade the worst case to O(n).

### Spaced revision (1h) — Weeks 7, 5, 2
| Problem | Source | Target time |
|---|---|---|
| LC 912 Sort an Array (merge sort, from scratch) | Week 7 (N−1) | under 20 min |
| LC 209 Minimum Size Subarray Sum | Week 2 (N−6) | under 15 min |

### Written editorial (1h)
**LC 410 Split Array Largest Sum.**

The observation to look for: *"Instead of searching for the split points, binary search the **answer** — the largest allowed subarray sum. For a candidate value, greedily count how many pieces you need; if that count is ≤ k the value is feasible, and feasibility is monotonic because a larger allowance never requires more pieces."*

The monotonicity argument is the part students omit. Insist on it — it is what makes the technique valid rather than lucky.

---

## Instructor notes

### What usually goes wrong this week
- **Two templates get blended.** Students mix `lo <= hi` with `hi = mid`, producing infinite loops. Insist on one template plus lower bound, each with its stated invariant.
- **"Binary search the answer" does not land on first exposure.** It looks like a trick until the third example. That is why LC 875 and LC 1011 are shown side by side, and why LC 1552 appears fresh in the contest.
- **Monotonicity goes unchecked.** Make "is it monotonic?" a required sentence in every submission this week.
- **Interval problems get the wrong sort key** and pass the sample tests anyway. The critique block is where to catch it.

### Watch list
Binary search failures are usually *precision* failures, not conceptual ones — which makes them very fixable. A student who cannot state the invariant is the one to help; a student who states it and slips on a boundary just needs reps.

### What to cut if you are behind
1. Thursday's flex (LC 1094) — it is in the weekend set
2. LC 452 from Thursday — LC 435 carries the same greedy
3. LC 81 and LC 2064 from the weekend set
4. Tuesday's flex (LC 33) — but it is a very common interview question, so prefer to cut elsewhere

**Never cut:** the invariant discussion, the four-step answer-space recipe, or the two diagnostic questions (*can I check feasibility fast? is it monotonic?*).
