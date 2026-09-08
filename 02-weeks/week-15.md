# Week 15 — Backtracking, Bit Manipulation & Design

**Phase III: Integration** · Difficulty band: **Medium/Hard** · Friday: **Mock Round 3 — instructor-conducted**

> The last week of new material. Three self-contained topics that each close a loop: **backtracking** is Week 11's DFS with an undo step; **bit manipulation** is a small toolkit of tricks worth memorising; **design problems** are Week 5's linked lists plus Week 1's hash maps, finally combined into LC 146 — a problem foreshadowed ten weeks ago.
>
> Friday's mock is the real thing: instructor-conducted, FAANG format, **passing bar 23/30**.

---

## Exit criteria

- [ ] Write the backtracking template from memory: choose, explore, un-choose
- [ ] Explain what distinguishes backtracking from plain DFS
- [ ] Handle duplicates in subsets/permutations/combinations correctly
- [ ] Use `x ^ x == 0`, `x & (x-1)` and `x & -x` and say what each does
- [ ] Design a class meeting stated complexity requirements by combining two structures
- [ ] Implement LRU Cache with O(1) get and put
- [ ] Score **23/30** in a full instructor-conducted mock interview

---

## Session 1 (Tuesday) — Backtracking

### Weekend-set debrief (0:00–0:15)
Debrief **LC 72 Edit Distance** — check whether the editorials named the three transitions and their source cells. Then **LC 673** and its second DP array.

### Concept spine (0:15–0:45)

**Part A — the connection to what they know.**

> **Backtracking is DFS over a tree of decisions, where you undo each choice on the way back out.**

*"You have been doing DFS since Week 9. The only new part is the undo."*

**The template. One template covers subsets, permutations, combinations, N-Queens and word search.**

```python
def backtrack(path, choices):
    if is_complete(path):
        result.append(path[:])       # COPY — path keeps mutating
        return
    for choice in choices:
        if not is_valid(choice, path):
            continue                 # PRUNE
        path.append(choice)          # 1. CHOOSE
        backtrack(path, next_choices(choice))   # 2. EXPLORE
        path.pop()                   # 3. UN-CHOOSE
    return
```

**Three things to make explicit:**

1. **`path[:]` — the copy.** Appending `path` itself stores a reference to a list that keeps changing, so every result ends up identical (usually empty). **This is the single most common backtracking bug**; demonstrate it live by omitting the copy.
2. **The un-choose must mirror the choose.** Whatever state you changed on the way in, restore on the way out.
3. **Pruning is where the performance is.** The template is exponential; `is_valid` is what makes it tractable.

**Part B — the decision tree.** Draw the tree for subsets of `[1,2,3]`:

```
                    []
         ┌──────────┼──────────┐
        [1]        [2]        [3]
     ┌───┴───┐      │
   [1,2]  [1,3]   [2,3]
     │
  [1,2,3]
```

*"Each level is one decision. The path from the root to any node is one partial solution."*

**Part C — the two shapes.**

| Shape | Mechanism | Example |
|---|---|---|
| **Subsets / combinations** | pass a start index so earlier elements are never revisited | LC 78, LC 39 |
| **Permutations** | all elements are always available; track which are used | LC 46 |

*"Order matters for permutations, so nothing is off-limits. Order does not matter for combinations, so a start index prevents counting the same set twice."*

**Part D — duplicates.** The hard part of this family, deferred to the weekend cluster.

> **Sort first, then skip a choice if it equals the previous one *at the same tree level*:** `if i > start and nums[i] == nums[i-1]: continue`.

State the rule now; LC 90, LC 40 and LC 47 in the weekend set drill it.

### Live-code (0:45–1:10)
**LC 78 Subsets** with the template verbatim, deliberately omitting `path[:]` first so the bug appears. Then **LC 46 Permutations** with a `used` array, contrasting the two shapes.

### Guided practice (1:15–1:50)
1. **LC 78 Subsets** — Medium
2. **LC 46 Permutations** — Medium
3. **LC 39 Combination Sum** — Medium. Reuse allowed → do not advance the start index.
4. **LC 22 Generate Parentheses** — Medium. Pruning is the whole problem: only add `)` when it would not exceed the open count.

LC 22 is the best pruning example in the course. Make sure everyone gets to it.

### Live critique (1:50–2:00)
An *LC 39*. Focus: does the recursive call pass `i` (reuse allowed) or `i + 1` (each item once)? A one-character difference that changes the problem entirely — and a good echo of Week 14's loop-direction lesson.

### Flex (2:00–2:30)
**LC 51 N-Queens** — Hard. The classic. Its interesting part is representing the constraints efficiently: three sets for columns and the two diagonals (`r + c` and `r − c` are constant along diagonals). In the weekend set; start it here.

### Common misconceptions
- Appending `path` instead of `path[:]`.
- Forgetting the un-choose, so state leaks between branches.
- Using a start index for permutations, or omitting it for combinations.
- Skipping duplicates globally instead of per level.
- Believing backtracking can be made polynomial. It cannot — pruning changes the constant and the practical size, not the class.

### Assignment 15.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 78 Subsets | Medium | 25 min |
| LC 46 Permutations | Medium | 25 min |
| LC 39 Combination Sum | Medium | 30 min |
| LC 22 Generate Parentheses | Medium | 25 min |
| LC 17 Letter Combinations of a Phone Number | Medium | 25 min |

---

## Session 2 (Wednesday) — Bit Manipulation

> A small toolkit. **Teach the tricks, not a theory** — this topic is worth about one session and no more.

### Warm-up (0:00–0:10)
1. State the three steps of the backtracking template.
2. Why `path[:]` and not `path`?
3. Start index or `used` array — permutations or combinations?

### Concept spine (0:10–0:45)

**Part A — the operations, each with the problem it solves.**

| Operation | Effect | Used for |
|---|---|---|
| `x ^ x == 0` | a value XORed with itself vanishes | find the unpaired element |
| `x ^ 0 == x` | XOR identity | the accumulator's starting value |
| `x & 1` | is x odd? | the lowest bit |
| `x >> 1` | divide by 2 | iterate over bits |
| `x & (x - 1)` | **clears the lowest set bit** | count set bits quickly |
| `x & -x` | **isolates the lowest set bit** | separate two unpaired values |
| `1 << i` | the i-th bit | subset masks |

**Part B — XOR, derived.** **LC 136 Single Number**, which they solved with a hash set in Week 1.

*"Every number appears twice except one. XOR everything together."*

Because XOR is commutative and associative, every pair cancels to 0, and `0 ^ single == single`.

> **O(n) time, O(1) space — better than the hash set they wrote in Week 1.** Show both side by side. Fourteen weeks apart, this is a satisfying upgrade.

**Part C — `x & (x - 1)`, derived not asserted.**

```
x     = 1011000
x - 1 = 1010111        subtracting 1 flips the lowest set bit and everything below it
x&x-1 = 1010000        so the lowest set bit is cleared
```

**Consequence:** counting set bits by repeatedly applying it loops once **per set bit**, not once per bit position. For sparse numbers that is much faster.

**Part D — `x & -x`.** In two's complement, `-x` is `~x + 1`, which leaves exactly the lowest set bit in common with `x`. Used in **LC 260 Single Number III**, where two values are unpaired: XOR everything (giving `a ^ b`), isolate any bit where they differ, and partition all numbers by that bit into two groups — each of which now has exactly one unpaired value.

**Part E — Python's caveat, which matters.** Python integers are arbitrary-precision and negatives have no fixed width. Problems assuming 32-bit integers need explicit masking:

```python
MASK = 0xFFFFFFFF
result &= MASK
if result > 0x7FFFFFFF:              # negative in 32-bit two's complement
    result = ~(result ^ MASK)
```

**JavaScript's bitwise operators coerce to 32-bit signed integers**, so JS students get this behaviour automatically — and must watch out when values exceed 2³¹.

### Live-code (0:45–1:10)
**LC 136** with XOR (contrasted with the Week 1 set solution), then **LC 338 Counting Bits** — where `dp[i] = dp[i >> 1] + (i & 1)` is DP meeting bit manipulation, a nice final combination.

### Guided practice (1:15–1:50)
1. **LC 191 Number of 1 Bits** — Easy. Both approaches; compare.
2. **LC 338 Counting Bits** — Easy
3. **LC 190 Reverse Bits** — Easy
4. **LC 260 Single Number III** — Medium

### Live critique (1:50–2:00)
An *LC 191*. Compare the shift-and-test loop (32 iterations always) with `x & (x-1)` (one iteration per set bit). Both O(1) for fixed width — a good discussion of when a constant factor is worth mentioning in an interview and when it is noise.

### Flex (2:00–2:30)
**LC 371 Sum of Two Integers** — add without `+`. XOR is addition without carry; `(a & b) << 1` is the carry; repeat until there is no carry. Elegant, and awkward in Python because of the masking — which is itself a useful lesson about the language.

### Common misconceptions
- Confusing `&` with `&&`, `|` with `||`.
- Operator precedence: `x & 1 == 0` parses as `x & (1 == 0)`. **Parenthesise.**
- Assuming 32-bit behaviour in Python.
- Believing bit tricks are always faster — they are usually about *space*, or about elegance.

### Assignment 15.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 191 Number of 1 Bits | Easy | 15 min |
| LC 338 Counting Bits | Easy | 20 min |
| LC 190 Reverse Bits | Easy | 20 min |
| LC 260 Single Number III | Medium | 35 min |
| LC 371 Sum of Two Integers | Medium | 30 min |

---

## Session 3 (Thursday) — Data Structure Design

> **The payoff session.** LC 146 was foreshadowed in Week 5 and everything needed for it has been taught.

### Warm-up (0:00–0:10)
1. What does `x & (x - 1)` do?
2. Why does XOR solve Single Number?
3. What does a doubly linked list give you that an array does not?

### Concept spine (0:10–0:45)

**Part A — the method, stated as a procedure.**

> **Read the required complexity for each operation. Ask which structure delivers each one. When no single structure delivers all of them, combine two.**

Put the table on the board:

| Need | Structure |
|---|---|
| O(1) lookup by key | hash map |
| O(1) insert/remove at a known position | doubly linked list |
| O(1) random access by index | array |
| O(log n) min/max | heap |
| Ordered iteration | sorted structure / BST |

**Part B — LC 146 LRU Cache, derived from requirements.**

> `get(key)` and `put(key, value)`, both **O(1)**. When full, evict the least recently used.

Walk it as a genuine derivation:

- *"O(1) get by key?"* → hash map.
- *"But we also need to know which key is least recently used, and to move a key to 'most recent' on every access. Can a hash map do that?"* → no, it has no order.
- *"An array ordered by recency?"* → lookup is O(1) by index, but moving an element to the front is O(n).
- *"A doubly linked list?"* → O(1) to splice a node out and re-insert at the head — **but only if you already have the node.**
- *"And what gives you the node in O(1)?"* → **the hash map, storing key → node.**

> **Hash map for lookup, doubly linked list for order. The map's values are node references, which is what makes the splice O(1).**

**Dummy head and tail nodes** remove every boundary case — Week 5's dummy-node lesson, cashed in.

This is the exact reasoning to perform aloud in an interview: **the structure is derived from the requirements, not recalled.** Model that explicitly.

**Part C — LC 380 Insert Delete GetRandom O(1).** Same method, different pair.
- `getRandom` in O(1) → an **array** (random index)
- `insert`/`delete` by value in O(1) → a **hash map** value → index
- The trick for O(1) delete: **swap the target with the last element, then pop.** Order is not required, so this is free.

*"Two problems, one method. That is what you are actually being tested on."*

### Live-code (0:45–1:15)
**LC 146 LRU Cache** in full, with dummy head and tail. This is a 30-minute build and the best possible use of the time — it composes classes (Week 5), hash maps (Week 1), doubly linked lists (Week 5) and the dummy-node idiom.

### Guided practice (1:20–1:50)
1. **LC 706 Design HashMap** — Easy. Build the structure the course has used since Week 1; chaining makes the collision story concrete.
2. **LC 380 Insert Delete GetRandom O(1)** — Medium
3. **LC 146 LRU Cache** — Medium

### Live critique (1:50–2:00)
An *LC 146*. Focus: dummy head and tail, or `None` checks everywhere? And does `get` also mark the entry as recently used? Forgetting that is the classic LRU bug and it passes naive tests.

### Flex (2:00–2:30)
**LC 355 Design Twitter** — Medium. Combines a hash map, a heap (Week 10) and a merge of k sorted feeds. A good closing demonstration that design problems are compositions of everything taught.

### Common misconceptions
- In LRU, forgetting that `get` counts as a use.
- Storing values rather than node references in the map, losing O(1) splicing.
- Not updating both the map and the list on eviction.
- In LC 380, doing an O(n) removal instead of swap-with-last.

### Assignment 15.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 706 Design HashMap | Easy | 30 min |
| LC 380 Insert Delete GetRandom O(1) | Medium | 35 min |
| LC 146 LRU Cache | Medium | 50 min |

---

## Session 4 (Friday) — ARENA: Mock Interview Round 3

**Mode:** **Instructor-conducted**, full FAANG format · **passing bar: 23/30 = would pass a real phone screen**

### Scheduling
14 students × 45 minutes does not fit in one session. **Anchor the round on Friday and spread the remainder across the week** — use Week 15's flex blocks and office hours. Friday's session hosts as many interviews as fit, plus the group debrief.

### Format — run it properly
- **Video call, or a shared editor with no autocomplete and no execution.** This is how most real rounds run, and coding without being able to run the code is a skill in itself.
- **You are a real interviewer:** neutral affect, no encouraging nods, no confirmation that an approach is right. This will feel unkind. It is the most useful thing you can do for them.
- **One Medium, then a follow-up that extends it.**
- Score the full 30-point rubric (`01-instructor/03-assessment-and-rubrics.md`, §3).

### Problem set (one Medium each, drawn across the whole course)
| Problem | Follow-up |
|---|---|
| LC 3 Longest Substring Without Repeating Characters | "Now return the substring itself." |
| LC 200 Number of Islands | "Now count only islands not touching the border." |
| LC 322 Coin Change | "Now return which coins were used." |
| LC 146 LRU Cache | "Now make it thread-safe — talk me through it." |
| LC 33 Search in Rotated Sorted Array | "What if there are duplicates?" |
| LC 102 Binary Tree Level Order Traversal | "Now do it with O(1) extra space besides the output." |
| LC 56 Merge Intervals | "What if the intervals arrive as a stream?" |
| LC 207 Course Schedule | "Now return a valid order." |
| LC 78 Subsets | "What if the input contains duplicates?" |
| LC 739 Daily Temperatures | "Prove it's O(n)." |
| LC 875 Koko Eating Bananas | "Why is the feasibility check monotonic?" |
| LC 143 Reorder List | "Do it without extra space." |
| LC 416 Partition Equal Subset Sum | "Now return the actual partition." |
| LC 973 K Closest Points to Origin | "Give me three approaches and compare them." |

### Written feedback — the highest-value hour you spend on any student
Give each student **half a page**: what would have passed, what would not, and the single most important thing to change. Return it before Week 16.

At 14 students this is roughly three hours of work. It is the most valuable three hours of the term.

### Group debrief
Compare Rounds 1, 2 and 3 as a cohort. Typical trajectory: clarification and narration improve a lot, testing improves somewhat, **complexity justification remains the weakest** — and that is what Week 16 targets.

### Assignment 15.4 (2h)
| Task | Budget |
|---|---|
| Read your written feedback and re-solve your problem addressing every point in it | 45 min |
| **Written:** compare your Round 1, 2 and 3 scores. What improved, what did not, and what is your plan for the final assessment? | 30 min |
| Solve **LC 79 Word Search** — Medium, backtracking on a grid | 45 min |

---

## Weekend Set 15 (6h) — due Tuesday, Week 16

### New problems (4h) — the duplicate-handling cluster
| Problem | Difficulty | Budget |
|---|---|---|
| LC 90 Subsets II | Medium | 40 min |
| LC 40 Combination Sum II | Medium | 40 min |
| LC 131 Palindrome Partitioning | Medium | 45 min |
| LC 47 Permutations II | Medium | 40 min |
| LC 51 N-Queens | Hard | 55 min |

**LC 90, LC 40 and LC 47 are deliberately clustered** — all three need duplicate skipping, and solving them back to back is what makes the per-level rule stick. Note that LC 47 (permutations) needs a slightly different guard than the two subset-style problems, which is exactly the discrimination worth drilling. **LC 131** combines backtracking with Week 4's palindrome check.

### Spaced revision (1h) — Weeks 14, 12, 9
| Problem | Source | Target time |
|---|---|---|
| LC 416 Partition Equal Subset Sum | Week 14 (N−1) | under 25 min |
| LC 743 Network Delay Time | Week 12 (N−3) | under 25 min |

### Written editorial (1h)
**LC 146 LRU Cache.**

The observation to look for: *"No single structure gives both O(1) lookup and O(1) reordering, so use two: a hash map from key to **node reference**, and a doubly linked list ordering nodes by recency. The map is what makes the list splice O(1) — without it you would have to search the list, which is O(n). Dummy head and tail nodes remove every boundary case."*

The phrase that matters is **"the map stores node references."** A student who says that has understood why the combination works rather than merely that it does — and that is precisely the reasoning a design interview is testing.

---

## Instructor notes

### What usually goes wrong this week
- **The `path[:]` bug hits everyone once.** Demonstrate it live rather than warning about it.
- **Bit manipulation feels arbitrary.** Keep it to one session and anchor every trick to a problem it solves. Do not go deeper — the return is low.
- **LC 146 is attempted from memory** by students who have seen it before. Push them to *derive* it from the complexity requirements, because that is what the interview actually assesses.
- **Mock Round 3 is emotionally heavy**, especially for students below the bar. Deliver the written feedback with a concrete plan attached, never as a verdict.

### Watch list
Round 3's scores are the closest thing you have to a real prediction of interview readiness. Sort the cohort by score and use that ordering to plan Week 16's Thursday weakness clinic — that session should be built directly from these results.

### What to cut if you are behind
1. Wednesday's flex (LC 371)
2. Thursday's flex (LC 355)
3. LC 190 from Wednesday's assignment
4. LC 51 from the weekend set — the duplicate cluster matters more

**Never cut:** the backtracking template with its live `path[:]` bug, the LC 146 derivation from requirements, the instructor-conducted mock, or the written feedback.
