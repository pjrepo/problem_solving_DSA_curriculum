# Week 1 — Toolkit & Complexity

**Phase I: Foundations** · Difficulty band: **Easy** · No algorithms this week — by design.

> This week teaches **no DSA**. It builds the two things every later week assumes: fluency with the collections that make interview solutions possible, and the vocabulary to talk about cost. Students arrive with loops and functions; a course that opens with "arrays and two pointers" would be teaching two-pointer technique to people who cannot yet reach for a set.

---

## Exit criteria

By Friday a student can:

- [ ] Choose between list, dict, set and tuple for a given job, and justify it
- [ ] Use `dict`, `set`, `Counter` and `defaultdict` fluently without looking them up
- [ ] State the time complexity of every operation they use on a list, dict or set
- [ ] Explain why `x in my_list` is O(n) but `x in my_set` is O(1)
- [ ] Give the Big-O of a loop nest they wrote, and justify it
- [ ] Apply the 5-step problem-solving frame to an unfamiliar problem

---

## Session 1 (Tuesday) — The Problem-Solving Frame & the Array/String Toolkit

### Opening (0:00–0:20) — course intro
No weekend set exists yet. Use this slot for the framing conversation in `00-overview/04-day-zero-setup.md`, §5. Be direct about the load, the Week 7 wall, and the rule that falling behind quietly is the only thing that will sink them.

Then review the day-zero *Two Sum* submissions **on the projector, anonymised**. You will have brute-force double loops, and possibly one hash-map solution. Do not judge any of them. Say: *"By Wednesday, all of you will know why the second one is better, and by Friday you'll know how to say that in interview language."*

### Concept spine (0:20–0:45)

**Part A — The 5-step frame.** Put this on the board. It stays on the board all term.

1. **Understand.** What is the input, exactly? What is the output? Write both down.
2. **Do it by hand.** Solve one small instance on paper. What did you actually do?
3. **Brute force.** Any working solution. Get it running.
4. **Find the waste.** What is it recomputing or re-scanning?
5. **Optimise.** What structure makes that fast?

Demonstrate on *Two Sum*: by hand on `[2,7,11,15], target=9` — "I looked at 2, then scanned for 7." Step 4: "I scan the whole array for each element." Step 5: is left as Wednesday's cliffhanger. **Do not resolve it today.**

**Part B — The four containers.** Not syntax — *decisions*.

| Need | Reach for | Why |
|---|---|---|
| Ordered items, index access | `list` | O(1) index, O(n) search |
| "Have I seen this?" | `set` | O(1) membership |
| "What's associated with this key?" | `dict` | O(1) lookup |
| Fixed group, usable as a dict key | `tuple` | immutable, hashable |

### Live-code (0:45–1:05)

Build a small "student marks" script live, using all four containers, narrating each choice:

```python
marks = [78, 92, 45, 92, 61]              # list: ordered, indexed
seen = set()                               # set: duplicate detection
by_subject = {"math": 78, "physics": 92}   # dict: keyed lookup
point = (3, 4)                             # tuple: hashable group
```

Then the operations they will use constantly. Type every one:

```python
arr[i]            arr[-1]           arr[1:4]         arr[::-1]
arr.append(x)     arr.pop()         arr.insert(0,x)  # <- point out this is O(n)
len(arr)          sorted(arr)       arr.sort()       # copy vs in-place
for i, v in enumerate(arr): ...
for a, b in zip(xs, ys): ...
s = "".join(parts)                 # NOT s += x in a loop
```

**Two things to make a point of:**
- `sorted()` returns a new list; `.sort()` mutates. Students confuse these all term.
- **String concatenation in a loop is O(n²)** because strings are immutable — each `+=` copies. Demonstrate it with `time`: building a 100,000-char string with `+=` versus `"".join()`. The visible difference is the hook for Thursday's complexity session.

**JavaScript deltas:** `arr.slice()` not `arr[1:4]` · `arr.length` not `len()` · `[...arr].reverse()` · **`arr.sort()` compares as strings by default** — `[10,9,1].sort()` gives `[1,10,9]`; you need `arr.sort((a,b) => a-b)`. This bites every JS student at least once.

### Guided practice (1:10–1:50)

Warm hands, no algorithms:

1. **LC 1480 Running Sum of 1d Array** — Easy
2. **LC 1929 Concatenation of Array** — Easy
3. **LC 344 Reverse String** — Easy, in place

Circulate. You are looking for who is fluent with indices and who is not. Note names.

### Live critique (1:50–2:00)
Take a Running Sum solution. Focus: naming and whether they mutated the input or built a new list — and whether that was a deliberate choice.

### Flex (2:00–2:30)
**LC 1672 Richest Customer Wealth** — first exposure to a 2D list. Then have students write, from memory, the list operations table from this session.

### Common misconceptions
- `arr2 = arr1` aliases; it does not copy. Demonstrate the bug live.
- `.sort()` returns `None`. Every student writes `arr = arr.sort()` once.
- Negative indexing and slice bounds — `arr[1:4]` excludes index 4.

### Assignment 1.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1480 Running Sum of 1d Array | Easy | 15 min |
| LC 1672 Richest Customer Wealth | Easy | 20 min |
| LC 1470 Shuffle the Array | Easy | 20 min |
| LC 26 Remove Duplicates from Sorted Array | Easy | 35 min |
| **Written:** the container decision table from memory, plus one sentence each on when you'd use it | — | 20 min |

---

## Session 2 (Wednesday) — Hash Maps & Sets

> **The most important session of Phase I.** Hash maps appear in more interview solutions than any other structure, and this cohort has never used one.

### Warm-up (0:00–0:10)
1. What does `sorted(arr)` return, and how does it differ from `arr.sort()`?
2. Why is building a long string with `+=` in a loop slow?
3. You have `[3,1,4,1,5]`. Write one line giving the unique values.

### Concept spine (0:10–0:40)

**Start with the pain.** Put the brute-force *Two Sum* on the board — the one their classmates wrote on day zero.

```python
for i in range(len(nums)):
    for j in range(i+1, len(nums)):
        if nums[i] + nums[j] == target: return [i, j]
```

Ask: *"For an array of 10,000 elements, how many pairs does this check?"* Get to ~50 million. *"What are we doing over and over?"* → **searching for a specific value in the array.**

*"What if searching were free?"*

**Now build the intuition for hashing.** Not the implementation — the bargain. A dict lets you jump straight to a value from its key, in constant time, by computing the key's hash to find its slot. The cost is memory, and the requirement is that keys are hashable (immutable).

Then resolve Tuesday's cliffhanger — the one-pass Two Sum:

```python
def two_sum(nums, target):
    seen = {}                      # value -> index
    for i, v in enumerate(nums):
        if target - v in seen:     # have I already passed my partner?
            return [seen[target - v], i]
        seen[v] = i
```

Sit on this. **The reframe — "instead of searching forward for my partner, I remember what I've passed and check if I *am* someone's partner" — is the single most transferable idea in Phase I.**

### Live-code (0:40–1:05)

The four tools, each with a problem it solves:

```python
from collections import Counter, defaultdict

counts = Counter("mississippi")        # {'i':4, 's':4, 'p':2, 'm':1}
counts.most_common(2)

groups = defaultdict(list)             # no KeyError on first access
for word in words:
    groups[tuple(sorted(word))].append(word)

seen = set()
if x in seen: ...                      # O(1)

d.get(k, 0)                            # default without a KeyError
for k, v in d.items(): ...
```

**Make the cost visible.** Run this in front of them:

```python
big_list = list(range(1_000_000))
big_set  = set(big_list)
# time: 999_999 in big_list   vs   999_999 in big_set
```

The gap is roughly four orders of magnitude. **This single demonstration does more to establish why data structures matter than any lecture.**

**JS deltas:** `Map` for non-string keys (a plain object coerces keys to strings — a real bug source) · `Set` · no `Counter`, so build with `map.set(k, (map.get(k) ?? 0) + 1)` · arrays cannot be Map keys by value, so use `arr.join(',')`.

### Guided practice (1:10–1:50)
1. **LC 217 Contains Duplicate** — Easy. The set pattern in three lines.
2. **LC 242 Valid Anagram** — Easy. Counter equality.
3. **LC 1 Two Sum** — Easy. They now write it properly.
4. **LC 387 First Unique Character in a String** — Easy. Two passes, one to count and one to scan.

### Live critique (1:50–2:00)
Take a *Valid Anagram*. Compare the sorting solution (O(n log n)) with the Counter solution (O(n)). **Both are correct; ask which they would give in an interview and why.** First taste of the idea that "it works" is not the end of the conversation.

### Flex (2:00–2:30)
**LC 49 Group Anagrams** — Medium, and the first genuinely non-obvious problem of the course. The insight is choosing the grouping key. Let them struggle; do not resolve if time runs out — it reappears in Week 2.

### Common misconceptions
- A `list` cannot be a dict key; a `tuple` can. Show the `TypeError`.
- `d[k]` on a missing key raises; `d.get(k)` returns `None`. Use `defaultdict` or `.get()`.
- Sets are unordered — do not rely on iteration order.
- Counting with a `dict` when `Counter` exists is not wrong, but is not idiomatic.

### Assignment 1.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 217 Contains Duplicate | Easy | 15 min |
| LC 242 Valid Anagram | Easy | 20 min |
| LC 1 Two Sum | Easy | 25 min |
| LC 349 Intersection of Two Arrays | Easy | 20 min |
| LC 136 Single Number | Easy | 25 min |
| **Written:** why is `x in set` faster than `x in list`? Three sentences. | — | 15 min |

---

## Session 3 (Thursday) — Time & Space Complexity

> The vocabulary for every remaining conversation in this course. Teach it as **counting**, not as mathematics.

### Warm-up (0:00–0:10)
1. Write the one-pass Two Sum from memory.
2. What does `Counter("aabbc")` return?
3. You need to check membership 10,000 times. List or set? Why?

### Concept spine (0:10–0:45)

**Do not open with a definition.** Open with the measurement from Wednesday — `in list` vs `in set` — and ask: *"How would we have predicted that without running it?"*

**Build up by counting operations:**

```python
def f(arr):            # 1 operation, regardless of size      -> O(1)
    return arr[0]

def g(arr):            # n operations                          -> O(n)
    for x in arr: print(x)

def h(arr):            # n * n                                 -> O(n²)
    for x in arr:
        for y in arr: print(x, y)

def k(arr):            # n + n, not n²  <- the one they get wrong
    for x in arr: print(x)
    for y in arr: print(y)
```

Then the three rules, derived rather than stated:
1. **Drop constants.** O(2n) → O(n). Ask why: at n = 1,000,000 the shape matters, the factor does not.
2. **Drop lower-order terms.** O(n² + n) → O(n²).
3. **Nested loops multiply; sequential loops add.**

**The growth table.** Put real numbers on the board — this is what makes it land:

| n | O(log n) | O(n) | O(n log n) | O(n²) | O(2ⁿ) |
|---|---|---|---|---|---|
| 10 | 3 | 10 | 33 | 100 | 1,024 |
| 1,000 | 10 | 1,000 | 9,966 | 1,000,000 | ~10³⁰¹ |
| 1,000,000 | 20 | 1,000,000 | 20,000,000 | 10¹² | — |

*"At a million elements, O(n) finishes instantly and O(n²) runs for days. This is why we do this course."*

**The interview shortcut** — teach this explicitly, it pays off immediately:

| Constraint given | Target complexity |
|---|---|
| n ≤ 20 | exponential is fine — O(2ⁿ) |
| n ≤ 1,000 | O(n²) |
| n ≤ 100,000 | O(n log n) |
| n ≤ 10,000,000 | O(n) or O(log n) |

*"The constraints tell you the intended solution before you have had an idea. Read them first, always."*

**Space complexity:** count *extra* memory, not the input. A hash map of n entries is O(n) space. The recursion call stack counts — flag this now, it matters in Week 7.

### Live-code (0:45–1:05)

Build the **complexity reference table** with the class (they copy it; it becomes `03-reference/05-complexity-reference.md`):

| Operation | list | set / dict |
|---|---|---|
| index / key access | O(1) | O(1) |
| membership `in` | **O(n)** | **O(1)** |
| append / add | O(1) amortised | O(1) |
| `insert(0, x)` / `pop(0)` | **O(n)** | — |
| delete by value | O(n) | O(1) |
| iterate | O(n) | O(n) |
| `sorted()` | O(n log n) | — |
| `min` / `max` / `sum` | O(n) | O(n) |

**Explain "amortised" properly** using `append`: the list occasionally reallocates and copies, but the cost spread across all appends is constant. This is the same argument used for monotonic stacks in Week 6 — plant it here.

### Guided practice (1:10–1:50)

**Part A — analyse, don't solve (20 min).** Six code snippets on a handout; students write the time and space complexity of each. Include the sequential-loops-are-O(n)-not-O(n²) trap, a loop whose bound is `n//2`, a nested loop where the inner runs `i` times (→ O(n²)), and a loop containing an `in list` check (the hidden O(n²)).

**Part B — apply (20 min).**
1. **LC 121 Best Time to Buy and Sell Stock** — Easy. Have them write the O(n²) first, *state its cost*, then improve it.
2. **LC 169 Majority Element** — Easy. Compare Counter O(n) with sorting O(n log n).

### Live critique (1:50–2:00)
A *Best Time to Buy and Sell Stock* solution. **The critique is entirely about the complexity claim in the header.** From this session on, a solution without a correct stated complexity is incomplete.

### Flex (2:00–2:30)
**LC 53 Maximum Subarray** — first exposure. Brute force O(n³), improve to O(n²), and stop there. Kadane's O(n) is Week 2's opener. Leaving it unresolved overnight is deliberate.

### Common misconceptions
- Two sequential loops are O(n), not O(n²). The most common error.
- "It has a nested loop so it's O(n²)" — not if the inner bound is constant.
- `if x in my_list` inside a loop is a hidden O(n²). **Show this one explicitly**; students write it constantly.
- O(n) is not "fast" — it is a growth *shape*. Constants can matter in practice; Big-O deliberately ignores them.
- Big-O is worst case unless stated otherwise.

### Assignment 1.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 121 Best Time to Buy and Sell Stock | Easy | 25 min |
| LC 169 Majority Element | Easy | 20 min |
| LC 268 Missing Number | Easy | 25 min |
| LC 448 Find All Numbers Disappeared in an Array | Easy | 30 min |
| **Written:** for each of the four above, state time *and* space complexity **with one sentence of justification each** | — | 20 min |

The written component is the point of this assignment. Grade it.

---

## Session 4 (Friday) — ARENA: Diagnostic Contest

**Mode:** Contest · **Purpose: baseline measurement, not assessment.** Say this out loud, twice.

### Format
90 minutes, 4 problems, individual, LeetCode submission. Full protocol in `01-instructor/02-arena-playbook.md`.

| Slot | Problem | Difficulty |
|---|---|---|
| P1 | LC 1929 Concatenation of Array | Easy |
| P2 | LC 217 Contains Duplicate | Easy |
| P3 | LC 1 Two Sum | Easy |
| P4 | LC 49 Group Anagrams | Medium |

**Deliberately gentle.** P1–P3 were all taught this week. The purpose is a clean baseline for every student, plus their first experience of the clock. **Every student should solve at least P1 and P2** — if anyone scores zero, that is a Week 1 intervention, not a Week 6 one.

### Reveal (1:45–2:00)
Leaderboard up. Frame it precisely:

> *"This is your baseline, and it is the worst you will ever perform in this room. We will run this same contest format nine more times. What matters is your curve, not today's position."*

Record every score. Contest 1's numbers are the comparison point for the whole term.

### Assignment 1.4 (2h)
| Task | Budget |
|---|---|
| Upsolve every contest problem you did not finish | 60 min |
| Write your first editorial on **LC 1 Two Sum** — full 7-part structure from `00-overview/02-weekly-rhythm.md`, §4 | 45 min |
| Set up `progress-tracker.md` with all Week 1 problems logged | 15 min |

**Model the editorial format explicitly** before they leave — show a complete worked example on the projector. The first one sets the standard for all sixteen.

---

## Weekend Set 1 (6h) — due Tuesday, Week 2

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 350 Intersection of Two Arrays II | Easy | 30 min |
| LC 283 Move Zeroes | Easy | 30 min |
| LC 125 Valid Palindrome | Easy | 35 min |
| LC 88 Merge Sorted Array | Easy | 40 min |
| LC 347 Top K Frequent Elements | Medium | 50 min |
| LC 238 Product of Array Except Self | Medium | 55 min |

The last two are a deliberate step up. LC 238 (no division, O(n)) is genuinely hard for week one — **tell them it is meant to be hard, and that reaching the editorial after 55 minutes is an acceptable outcome this week.**

### Spaced revision (1h)
No prior weeks yet. Instead: **re-solve LC 1 Two Sum and LC 242 Valid Anagram from a blank file, timed.** Record both times in the tracker. These become the first entries in the personal baseline.

### Written editorial (1h)
**LC 347 Top K Frequent Elements.** Full 7-part structure.

Push hard on part 3, *the key observation*. Most students will write "use a Counter." That is not the observation — the observation is that **you never need to sort all n distinct values when you only need k of them.** Whether they see this is your first real read on who is understanding versus pattern-matching.

---

## Instructor notes

### What usually goes wrong this week
- **Setup eats Session 1.** Prevent it — enforce day zero properly.
- **Students find Week 1 easy and get complacent.** Say plainly: *"This week is the vocabulary. The course starts on Tuesday."*
- **Complexity feels pointless** because at n = 10 everything is instant. The million-element `in list` vs `in set` demo is the entire fix. Do not skip it.
- **A quiet student who cannot use a dict by Friday** is your first intervention. This will not fix itself.

### Watch list
The day-zero *Two Sum* submissions plus Session 1's guided practice give you a fast read on baseline syntax fluency. Anyone who struggled to index an array on Tuesday needs watching from now, not from Week 6.

### What to cut if you are behind
1. All three flex blocks (LC 1672, LC 49, LC 53)
2. LC 387 from Wednesday's guided practice
3. Thursday's guided practice Part A down to three snippets from six

**Never cut:** the `in list` vs `in set` timing demonstration, the one-pass Two Sum derivation, or the interview constraint table. Those three are what Week 1 exists to deliver.
