# Week 2 — Arrays I: Traversal, Prefix Sums, Counting

**Phase I: Foundations** · Difficulty band: **Easy → Easy/Medium**

> The first week of actual algorithms. Three ideas that recur constantly: scanning while maintaining state, precomputing to make range queries free, and using a hash map to turn a search into a lookup. Kadane's and prefix sums are also the two places where students first meet the shape of dynamic programming — say so, quietly, so Week 13 feels like a return rather than an arrival.

---

## Exit criteria

- [ ] Write a single-pass scan that maintains running state (max, min, sum) without a second loop
- [ ] Derive Kadane's algorithm from the brute force, and explain the "extend or restart" decision
- [ ] Build a prefix-sum array and answer any range-sum query in O(1)
- [ ] Recognise "how many subarrays sum to k" as prefix sums + hash map
- [ ] Choose the right grouping key for a counting problem
- [ ] State the time and space complexity of every solution they write, unprompted

---

## Session 1 (Tuesday) — Single-Pass Traversal & Kadane's

### Weekend-set debrief (0:00–0:15)
Read the 14 submissions Tuesday morning. Debrief the two that hurt: **LC 238 Product of Array Except Self** (the prefix/suffix idea, which sets up Wednesday perfectly) and **LC 347 Top K Frequent** (heap vs bucket sort — foreshadow Week 10).

### Concept spine (0:15–0:45)

**Part A — the single-pass mindset.** Most beginners write two loops: one to find something, one to use it. Often one pass suffices if you carry state.

```python
# Two passes                          # One pass
mx = max(arr)                          mx = arr[0]
idx = arr.index(mx)                    idx = 0
                                       for i, v in enumerate(arr):
                                           if v > mx: mx, idx = v, i
```

*"Both are O(n). But the one-pass version generalises — the moment your condition depends on what you've already seen, two passes stops working."*

**Part B — Kadane's, derived.** Resume Thursday's unfinished LC 53.

Brute force O(n²): try every start, extend to every end, track the best.

Ask: *"When you're standing at index i, having built a running sum, what are your two options?"*
→ **extend the current subarray, or start a new one at i.**

*"When is starting fresh better?"* → when the running sum so far is negative, since it can only drag you down.

```python
def max_subarray(nums):
    best = cur = nums[0]
    for v in nums[1:]:
        cur = max(v, cur + v)      # restart, or extend
        best = max(best, cur)
    return best
```

**Say this out loud:** *"You have just written a dynamic programming solution. `cur` is the best subarray ending at this index. In Week 13 we'll give this a name; for now, notice that the trick was defining what we're tracking precisely."*

That framing does more for Week 13 than any DP lecture will.

### Live-code (0:45–1:05)
Kadane's from scratch, then extend to **return the indices** as well — which forces students to think about *when* to update the start pointer, and is a classic follow-up.

Then **LC 152 Maximum Product Subarray**, at least partially: negatives flip the ordering, so you must track both the running max *and* the running min. Good demonstration that a pattern requires thought, not recall.

**JS delta:** `Math.max(...arr)` blows the stack on very large arrays — use a reduce or a loop.

### Guided practice (1:10–1:50)
1. **LC 53 Maximum Subarray** — Medium (they just derived it; make them write it unaided)
2. **LC 121 Best Time to Buy and Sell Stock** — Easy. *Ask them to notice it is the same shape as Kadane's.*
3. **LC 485 Max Consecutive Ones** — Easy. Running state, trivial version.
4. **LC 1207 Unique Number of Occurrences** — Easy. Counting revision from Week 1.

### Live critique (1:50–2:00)
A Kadane's solution. Look for: initialising `best = 0` (breaks on all-negative arrays — a genuine bug, catch it), and whether `best` is updated inside or outside the loop.

### Flex (2:00–2:30)
**LC 152 Maximum Product Subarray** in full. Genuinely tricky, and a good stretch problem for the top of the room.

### Common misconceptions
- Initialising `best = 0` instead of `arr[0]`. Fails on `[-3,-1,-2]`. **The most common bug this week — make it the critique focus.**
- Confusing "maximum subarray" (contiguous) with "maximum subsequence" (not). Define both terms explicitly.
- Trying to track the start index by resetting at the wrong moment.

### Assignment 2.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 53 Maximum Subarray | Medium | 30 min |
| LC 485 Max Consecutive Ones | Easy | 15 min |
| LC 643 Maximum Average Subarray I | Easy | 25 min |
| LC 122 Best Time to Buy and Sell Stock II | Medium | 30 min |
| LC 152 Maximum Product Subarray | Medium | 35 min |

---

## Session 2 (Wednesday) — Prefix Sums & Difference Arrays

### Warm-up (0:00–0:10)
1. Write Kadane's from memory.
2. Why does `best = 0` break on all-negative input?
3. What is the complexity of `sum(arr[i:j])` inside a loop over all `i`?

Question 3 is the setup for today. Let them arrive at O(n³) or O(n²) themselves.

### Concept spine (0:10–0:40)

**Start with the waste.** *"Sum from index 2 to 7. Now 3 to 9. Now 2 to 8. What are we doing repeatedly?"* → re-adding the same middle elements.

**The idea:** precompute cumulative sums once.

```
arr     =  [3, 1, 4, 1, 5, 9]
prefix  = [0, 3, 4, 8, 9, 14, 23]     # prefix[i] = sum of first i elements
sum(i..j) = prefix[j+1] - prefix[i]
```

**Insist on the leading zero.** `prefix[0] = 0` makes the formula uniform and removes every off-by-one special case. Students who skip it write bug-prone code all term.

O(n) build, then **O(1) per query, forever**.

**Difference arrays — the inverse.** For repeated *range updates*: mark `+v` at the start and `−v` just past the end, then take a prefix sum at the finish. Range update becomes O(1); one final O(n) pass reconstructs the array.

**The composite that matters most: prefix sums + hash map.** For LC 560 (count subarrays summing to k):

> `sum(i..j) == k` is the same as `prefix[j+1] - prefix[i] == k`, which is the same as `prefix[i] == prefix[j+1] - k`.

*"So as we scan, we ask: how many earlier prefixes had the value `current - k`? That is a hash map lookup."*

**Point at the board:** this is exactly the Two Sum reframe from Week 1 — instead of searching forward, remember what you have passed. Same idea, harder disguise. **This connection is the most valuable thing said this week.**

### Live-code (0:40–1:05)
1. `build_prefix(arr)` and a `range_sum(i, j)` helper
2. **LC 560 Subarray Sum Equals K** in full, narrating the reframe

```python
def subarray_sum(nums, k):
    counts = {0: 1}          # empty prefix — the line everyone forgets
    total = ans = 0
    for v in nums:
        total += v
        ans += counts.get(total - k, 0)
        counts[total] = counts.get(total, 0) + 1
    return ans
```

Dwell on `{0: 1}`. It handles subarrays starting at index 0. Omitting it is *the* classic bug — show the failure on `[3], k=3`.

### Guided practice (1:10–1:50)
1. **LC 303 Range Sum Query - Immutable** — Easy. The pattern, bare.
2. **LC 724 Find Pivot Index** — Easy. Prefix from both directions.
3. **LC 1480 Running Sum of 1d Array** — Easy. Week 1 revision, now with a name.
4. **LC 560 Subarray Sum Equals K** — Medium. The payoff.

### Live critique (1:50–2:00)
A *Find Pivot Index*. Focus: did they build two prefix arrays, or realise one plus the total suffices? A good, concrete conversation about space.

### Flex (2:00–2:30)
**LC 238 Product of Array Except Self** revisited from the weekend, now framed as prefix/suffix products. Students who fought it on Saturday will see it snap into place — that experience is worth engineering.

### Common misconceptions
- Off-by-one in `prefix[j+1] - prefix[i]`. The leading zero is the cure.
- Forgetting `{0: 1}` in LC 560.
- Trying to use prefix sums when the array is being modified — flag that this is what segment trees are for, and that **we are deliberately not covering them.**

### Assignment 2.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 303 Range Sum Query - Immutable | Easy | 20 min |
| LC 724 Find Pivot Index | Easy | 25 min |
| LC 560 Subarray Sum Equals K | Medium | 40 min |
| LC 523 Continuous Subarray Sum | Medium | 35 min |

---

## Session 3 (Thursday) — Hash-Map Counting Patterns

### Warm-up (0:00–0:10)
1. Why does LC 560 need `{0: 1}` in the map?
2. Give the prefix-sum formula for `sum(i..j)`.
3. `Counter("aabbbc").most_common(1)` → ?

### Concept spine (0:10–0:40)

Week 1 introduced the hash map as a container. Today it becomes a **technique**, in four recognisable shapes:

| Shape | Question it answers | Canonical |
|---|---|---|
| **Frequency** | how many times does each element appear? | LC 387, LC 1207 |
| **Grouping** | which items share a property? | LC 49 Group Anagrams |
| **Seen-set** | have I encountered this before? | LC 217, LC 141 |
| **Complement** | does my partner exist? | LC 1 Two Sum, LC 560 |

**Grouping deserves the most time**, because *choosing the key* is the whole problem.

For LC 49: two candidate keys — the sorted string (`"eat"` → `"aet"`), or a 26-tuple of letter counts. Both work. Compare the costs honestly: sorting each word is O(k log k); counting is O(k). For long words the tuple key wins.

**Make the general point:** *"When grouping, ask — what makes two items 'the same'? Encode exactly that, and nothing else, as the key."* That question transfers everywhere.

### Live-code (0:40–1:05)
**LC 49 Group Anagrams** both ways, side by side, comparing complexity. Then **LC 347 Top K Frequent Elements** — Counter, then bucket sort by frequency, which achieves O(n) and previews Week 10's heap discussion.

**JS delta:** objects coerce keys to strings, so `{}` with a tuple key silently breaks. Use `Map` with a joined string key, and say why.

### Guided practice (1:10–1:50)
1. **LC 49 Group Anagrams** — Medium
2. **LC 383 Ransom Note** — Easy. Counter subtraction.
3. **LC 1010 Pairs of Songs With Total Durations Divisible by 60** — Medium. Complement with a modulus twist — an excellent transfer test.
4. **LC 219 Contains Duplicate II** — Easy. Seen-map storing indices.

Problem 3 is the one to watch. It is Two Sum wearing a disguise; who spots that tells you who is learning patterns rather than problems.

### Live critique (1:50–2:00)
An *LC 1010*. Focus on whether they saw the complement structure or brute-forced pairs. Ask the author: *"What made you think of a hash map here?"* The answer reveals whether the trigger signal has landed.

### Flex (2:00–2:30)
**LC 128 Longest Consecutive Sequence** — Medium. Requires the non-obvious "only start counting from a number whose predecessor is absent" insight to reach O(n). Excellent stretch.

### Common misconceptions
- Sorting when counting would do — O(n log n) where O(n) exists.
- Using a list as a dict key → `TypeError`. Convert to tuple.
- In "grouping", building the key inconsistently across items.
- Believing "I used a hash map" means "it is O(n)" — it does not, if you are also sorting inside the loop.

### Assignment 2.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 49 Group Anagrams | Medium | 35 min |
| LC 383 Ransom Note | Easy | 15 min |
| LC 219 Contains Duplicate II | Easy | 20 min |
| LC 1010 Pairs of Songs Divisible by 60 | Medium | 35 min |
| LC 451 Sort Characters By Frequency | Medium | 30 min |

---

## Session 4 (Friday) — ARENA: Contest 1

**Mode:** Contest · 90 minutes · full protocol in `01-instructor/02-arena-playbook.md`

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 1512 Number of Good Pairs | Easy | Hash-map counting |
| P2 | LC 53 Maximum Subarray | Medium | Kadane's — taught Tuesday |
| P3 | LC 560 Subarray Sum Equals K | Medium | Prefix + hash map — the week's hardest idea |
| P4 | LC 454 4Sum II | Medium | Transfer — Two Sum generalised, never taught |

**Calibration:** every student should get P1 and P2. P3 separates those who understood Wednesday from those who copied it. P4 is for the top three.

### Reveal (1:45–2:00)
P3 is the one to explain properly. Re-derive the `prefix[i] == current - k` reframe on the board and connect it, again, to Two Sum. **Repetition of this specific link across three sessions is deliberate** — it is the seed of pattern-transfer thinking.

### Assignment 2.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 560 Subarray Sum Equals K** | 45 min |
| Update `progress-tracker.md`, including any red-list entries | 15 min |

---

## Weekend Set 2 (6h) — due Tuesday, Week 3

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1732 Find the Highest Altitude | Easy | 20 min |
| LC 209 Minimum Size Subarray Sum | Medium | 45 min |
| LC 525 Contiguous Array | Medium | 50 min |
| LC 974 Subarray Sums Divisible by K | Medium | 45 min |
| LC 128 Longest Consecutive Sequence | Medium | 45 min |
| LC 41 First Missing Positive | Hard | 50 min |

**LC 525** and **LC 974** are both LC 560 in disguise — a deliberate cluster so the pattern generalises rather than staying attached to one problem. **LC 209** is a soft on-ramp to Week 3's sliding window. **LC 41** is a genuine Hard; tell them it is a stretch and that reading the editorial after 50 minutes is acceptable.

### Spaced revision (1h) — from Week 1 (timed baseline)
| Problem | Source | Target time |
|---|---|---|
| LC 242 Valid Anagram | Week 1 | under 10 min |
| LC 347 Top K Frequent Elements | Week 1 | under 20 min |

**Re-solve from a blank file.** Log the time. If either takes as long as the first attempt, it goes on the red list.

### Written editorial (1h)
**LC 525 Contiguous Array.**

The key observation to look for: *"Treat 0 as −1; then equal counts of 0 and 1 means a subarray sum of zero — so this is LC 560 with k = 0."* A student who writes that has genuinely generalised. A student who describes the code line by line has not, and should be flagged.

---

## Instructor notes

### What usually goes wrong this week
- **Prefix sums feel pointless** until LC 560. Do not teach them abstractly for 20 minutes first — go to the payoff quickly.
- **The `{0: 1}` bug** will hit most of the class. Let it happen in guided practice rather than pre-empting it; the failure teaches better than the warning.
- **Kadane's `best = 0` bug** appears in roughly half the submissions. Make it the critique focus.
- **Students treat LC 525/974/1248 as three new problems** rather than one pattern. If the weekend editorials do not show the generalisation, spend Week 3's Tuesday debrief on it.

### Watch list
This is the week where the gap between "I can code" and "I can solve" opens up. Students fluent in Week 1 may stall here — that is normal and not yet a concern. What *is* a concern: a student who cannot explain their own working solution.

### What to cut if you are behind
1. Flex blocks (LC 152, LC 238 revisit, LC 128)
2. Difference arrays from Wednesday — keep prefix sums, drop the inverse; nothing later depends on it
3. LC 219 and LC 383 from the daily sets
4. Weekend set from 6 problems to 4 — drop LC 41 and LC 974

**Never cut:** the LC 560 derivation, or the explicit "this is Two Sum again" connection.
