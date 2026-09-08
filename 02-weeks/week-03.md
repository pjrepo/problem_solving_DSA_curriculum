# Week 3 — Two Pointers & Sliding Window

**Phase I: Foundations** · Difficulty band: **Easy/Medium**

> The highest-yield week in Phase I. Two pointers and sliding window between them cover a large share of array and string interview questions, and both replace an O(n²) scan with a single O(n) pass. The week's real lesson is not the two templates — it is learning to ask *"can I avoid re-examining what I have already seen?"*

---

## Exit criteria

- [ ] Recognise the trigger for opposite-end two pointers (sorted input + find a pair/triplet) and write it without reference
- [ ] Explain **why** sortedness lets you discard half the candidates each step
- [ ] Handle duplicate-skipping in 3Sum correctly
- [ ] Write the read/write pointer template for in-place array modification
- [ ] Write the **variable sliding window** template from memory, and explain why it is O(n) despite the inner loop
- [ ] Distinguish fixed-size from variable-size window problems from the statement alone

---

## Session 1 (Tuesday) — Two Pointers: Opposite Ends

### Weekend-set debrief (0:00–0:15)
Debrief **LC 525 Contiguous Array** — check whether the editorials showed the 0→−1 generalisation. If most did not, re-derive it now; it is the difference between learning a pattern and memorising a problem.

Also flag **LC 209 Minimum Size Subarray Sum** from the weekend: *"Several of you solved that with two pointers moving in the same direction. That has a name, and it is Thursday's session."*

### Concept spine (0:15–0:45)

**Start sorted.** `[2, 7, 11, 15]`, find a pair summing to 18.

Brute force: every pair, O(n²). Now put one finger at each end.

```
[2, 7, 11, 15]     2 + 15 = 17 < 18  → too small. Which finger moves?
 L          R
```

Get the room to answer. Moving `R` left only *decreases* the sum, so it cannot help — **`L` must move right.**

**This is the whole idea, and it deserves to be stated as a principle:**
> Because the array is sorted, each comparison tells us which pointer *cannot possibly* be part of the answer. We discard it and never look again — so each element is visited once.

**Sortedness is the enabling condition.** Without it the comparison tells you nothing. Make students say this back.

**Then 3Sum**, which is the real content of the session. Fix one element, two-pointer the rest:

```python
def three_sum(nums):
    nums.sort()
    res = []
    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i-1]:      # skip duplicate anchors
            continue
        lo, hi = i + 1, len(nums) - 1
        while lo < hi:
            s = nums[i] + nums[lo] + nums[hi]
            if s < 0:   lo += 1
            elif s > 0: hi -= 1
            else:
                res.append([nums[i], nums[lo], nums[hi]])
                while lo < hi and nums[lo] == nums[lo+1]: lo += 1   # skip dup
                while lo < hi and nums[hi] == nums[hi-1]: hi -= 1
                lo += 1; hi -= 1
    return res
```

**Be explicit:** the two-pointer part is easy; **the duplicate handling is 80% of the difficulty and 100% of the failed submissions.** Walk `[-2,0,0,2,2]` by hand on the board, slowly.

Complexity: O(n log n) sort + O(n²) scan = **O(n²)**, which beats the O(n³) brute force.

### Live-code (0:45–1:05)
**LC 167 Two Sum II** first (60 seconds, builds confidence), then **LC 11 Container With Most Water** — where the greedy insight is genuinely non-obvious: always move the pointer at the *shorter* wall, because moving the taller one can never increase the area. Have students argue why before you confirm.

### Guided practice (1:10–1:50)
1. **LC 167 Two Sum II** — Easy
2. **LC 977 Squares of a Sorted Array** — Easy. Two pointers from the outside in; negatives make it interesting.
3. **LC 680 Valid Palindrome II** — Easy. One deletion allowed; forces a branch.
4. **LC 11 Container With Most Water** — Medium

### Live critique (1:50–2:00)
A *Container With Most Water*. Focus: can the author **justify** moving the shorter wall? Working code with an unjustified greedy choice is exactly what gets probed in interviews.

### Flex (2:00–2:30)
**LC 15 3Sum** in full, with the duplicate handling.

### Common misconceptions
- Applying opposite-end two pointers to an **unsorted** array. Ask "what does the comparison tell you?" — nothing.
- In 3Sum, skipping duplicates for `lo`/`hi` but not for the anchor `i` (or vice versa).
- `while lo <= hi` instead of `lo < hi`, producing a pair with itself.
- Believing the sort makes it O(n log n) overall — the nested scan dominates at O(n²).

### Assignment 3.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 167 Two Sum II | Easy | 15 min |
| LC 977 Squares of a Sorted Array | Easy | 25 min |
| LC 680 Valid Palindrome II | Easy | 25 min |
| LC 11 Container With Most Water | Medium | 25 min |
| LC 15 3Sum | Medium | 45 min |

---

## Session 2 (Wednesday) — Two Pointers: Same Direction & Partitioning

### Warm-up (0:00–0:10)
1. Why must the array be sorted for opposite-end two pointers?
2. In *Container With Most Water*, why move the shorter wall?
3. What is 3Sum's complexity, and which part dominates?

### Concept spine (0:10–0:40)

**The read/write pattern.** *"You wrote this in Week 1 without knowing it had a name."* Put LC 26 (Remove Duplicates) on the board — most of them solved it on day two.

```python
def remove_duplicates(nums):
    write = 1                                  # where the next kept element goes
    for read in range(1, len(nums)):           # what we're examining
        if nums[read] != nums[write - 1]:
            nums[write] = nums[read]
            write += 1
    return write
```

Name the roles clearly: **`read` scans everything; `write` marks the frontier of the kept prefix.** Once students have those two nouns, the entire family becomes mechanical.

**The invariant** — and this is the transferable idea:
> Everything before `write` is finished and correct. Everything from `read` onwards is unexamined.

Ask them to state the invariant for every problem in this family before coding. It is the same discipline that makes binary search reliable in Week 8.

**Then partitioning — Dutch national flag (LC 75).** Three regions instead of two:

```
[ 0s | 1s | unexamined | 2s ]
      lo   mid        hi
```

Walk it by hand. The trap: when you swap with `hi`, **do not advance `mid`** — the element you just swapped in is unexamined. Every student hits this; let them hit it in the lab.

**Fast & slow pointers** (a short preview, ~5 min): two pointers at different speeds. On an array, LC 287 finds a duplicate in O(1) space. The full treatment is Week 5, on linked lists — say so.

### Live-code (0:40–1:05)
**LC 27 Remove Element** (2 min), then **LC 75 Sort Colors** in full with the mid/hi trap demonstrated live.

### Guided practice (1:10–1:50)
1. **LC 27 Remove Element** — Easy
2. **LC 905 Sort Array By Parity** — Easy. Two-region partition.
3. **LC 80 Remove Duplicates from Sorted Array II** — Medium. At most twice; forces the invariant to be stated precisely.
4. **LC 75 Sort Colors** — Medium

### Live critique (1:50–2:00)
An *LC 80*. Focus: did they generalise (`nums[read] != nums[write-2]`) or special-case with a counter? Both work; the generalised form is far better, and comparing them is a good conversation about how a clean invariant produces clean code.

### Flex (2:00–2:30)
**LC 287 Find the Duplicate Number** — Medium. Floyd's cycle detection applied to an array. Genuinely surprising, and it sets up Week 5.

### Common misconceptions
- Advancing `mid` after swapping with `hi` in Dutch national flag.
- Using an extra output array when the problem says in place.
- Off-by-one in `write` initialisation — should it start at 0 or 1?
- Forgetting that the elements past the returned length may be anything.

### Assignment 3.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 27 Remove Element | Easy | 15 min |
| LC 905 Sort Array By Parity | Easy | 20 min |
| LC 443 String Compression | Medium | 35 min |
| LC 80 Remove Duplicates from Sorted Array II | Medium | 30 min |
| LC 75 Sort Colors | Medium | 30 min |

---

## Session 3 (Thursday) — Sliding Window

> The most reusable template in the course. Students will use this in Weeks 3, 4, and again in Week 14.

### Warm-up (0:00–0:10)
1. State the read/write invariant.
2. Why don't you advance `mid` after a swap with `hi` in Sort Colors?
3. `[1,2,3,4]`, windows of size 2 — how many, and what is the sum of each?

### Concept spine (0:10–0:45)

**Fixed window first (easy).** Maximum sum of any 4 consecutive elements. Brute force recomputes each window: O(n·k). *"When the window slides right by one, what actually changed?"* → one element in, one out. Subtract and add. O(n).

**Variable window (the real content).** *"Longest substring with no repeating characters."* The window is no longer a fixed size — it grows and shrinks based on a **validity condition**.

Derive the template with them, on the board:

```python
def longest_valid_window(s):
    window = {}                       # whatever state defines validity
    left = 0
    best = 0
    for right, ch in enumerate(s):
        # 1. EXPAND: add s[right] to the window state
        window[ch] = window.get(ch, 0) + 1

        # 2. SHRINK: while the window is INVALID, move left forward
        while window[ch] > 1:
            window[s[left]] -= 1
            left += 1

        # 3. RECORD: the window is now valid
        best = max(best, right - left + 1)
    return best
```

**Three steps, always in this order: expand, shrink-while-invalid, record.** Every variable window problem in the course is this template with a different definition of "invalid".

**The complexity argument — teach it properly, it is the lesson.** *"There is a `while` loop inside a `for` loop. Is this O(n²)?"* Let them think. No: **`left` only ever moves forward, so across the entire run it advances at most n times total.** Each element enters the window once and leaves once → O(n).

This is the same amortised argument as `list.append` in Week 1 and the monotonic stack in Week 6. **Name the connection.**

**Recognition table** — put this on the board:

| Statement says | Window type |
|---|---|
| "subarray of size k" | Fixed |
| "longest/shortest substring such that…" | Variable |
| "at most k distinct / at most k replacements" | Variable, shrink while the count exceeds k |
| "contains all characters of t" | Variable, shrink while still valid (minimising) |

**Note the inversion for minimising problems:** when finding the *shortest* valid window you shrink while the window is *valid*, recording as you go. Flag it now; Week 4 uses it for LC 76.

### Live-code (0:45–1:05)
**LC 3 Longest Substring Without Repeating Characters** from scratch, using the three-step template verbatim. Then **LC 1004 Max Consecutive Ones III**, where "invalid" means "more than k zeros" — the same template, one line changed. Showing that one line is what makes the template feel like a tool rather than a memorised solution.

### Guided practice (1:10–1:50)
1. **LC 643 Maximum Average Subarray I** — Easy. Fixed window; Week 2 revision with a name attached.
2. **LC 1456 Maximum Number of Vowels in a Substring of Given Length** — Medium. Fixed window.
3. **LC 3 Longest Substring Without Repeating Characters** — Medium
4. **LC 1004 Max Consecutive Ones III** — Medium

### Live critique (1:50–2:00)
An *LC 3*. Focus: did they follow the expand/shrink/record order, or improvise? Improvised window code usually works on the samples and fails on an edge case — a good, concrete argument for using a template.

### Flex (2:00–2:30)
**LC 424 Longest Repeating Character Replacement** — Medium, and the hardest window idea of the week: the window is valid when `(window length − count of the most frequent character) ≤ k`. Worth the full 30 minutes.

### Common misconceptions
- Using `if` instead of `while` for the shrink step. Works on the samples, fails when multiple shrinks are needed.
- Recording the answer *before* restoring validity.
- Forgetting to remove `s[left]` from the window state when advancing `left`.
- Claiming O(n²) because of the nested loop. Make them state the amortised argument.

### Assignment 3.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1456 Maximum Number of Vowels in a Substring | Medium | 25 min |
| LC 3 Longest Substring Without Repeating Characters | Medium | 30 min |
| LC 1004 Max Consecutive Ones III | Medium | 30 min |
| LC 424 Longest Repeating Character Replacement | Medium | 35 min |

---

## Session 4 (Friday) — ARENA: Contest 2

**Mode:** Contest · 90 minutes

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 392 Is Subsequence | Easy | Same-direction two pointers, fresh |
| P2 | LC 187 Repeated DNA Sequences | Medium | Fixed window + set |
| P3 | LC 16 3Sum Closest | Medium | Transfer — 3Sum variant, never taught |
| P4 | LC 42 Trapping Rain Water | Hard | Two-pointer stretch |

**Calibration:** P1 for everyone. P3 is the real test — students who learned *3Sum* will struggle; students who learned *opposite-end two pointers* will adapt. P4 will be solved by one or two.

### Reveal (1:45–2:00)
Spend the time on **P3**, framed exactly that way: *"If you had memorised 3Sum you were stuck. If you understood why the pointers move, you weren't."* This is the single most important thing to say all week.

### Assignment 3.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 42 Trapping Rain Water** (whether or not you solved it) | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 3 (6h) — due Tuesday, Week 4

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 567 Permutation in String | Medium | 35 min |
| LC 438 Find All Anagrams in a String | Medium | 35 min |
| LC 713 Subarray Product Less Than K | Medium | 40 min |
| LC 611 Valid Triangle Number | Medium | 40 min |
| LC 18 4Sum | Medium | 45 min |
| LC 986 Interval List Intersections | Medium | 40 min |

**LC 567 and LC 438 are the same problem** — a fixed window with a character-count comparison. Solve 567 first, then notice. **LC 18** generalises 3Sum by one level, which is the real test of whether the pattern (rather than the problem) was learned. **LC 986** previews Week 8's interval work.

### Spaced revision (1h) — from Week 2 (only N−1 exists this early)
| Problem | Source | Target time |
|---|---|---|
| LC 560 Subarray Sum Equals K | Week 2 | under 20 min |
| LC 128 Longest Consecutive Sequence | Week 2 | under 20 min |

Re-solve from blank files. Anything that takes as long as the first attempt goes on the red list.

### Written editorial (1h)
**LC 713 Subarray Product Less Than K.**

The observation to look for: *"When the window `[left..right]` is valid, it contributes `right − left + 1` new subarrays — every suffix of the window ending at `right`."* Counting-window problems are a distinct sub-family, and whether a student articulates that counting rule tells you clearly whether they understand windows or are copying a shape.

---

## Instructor notes

### What usually goes wrong this week
- **3Sum duplicate handling.** Expect roughly half the class to fail on `[0,0,0,0]`. Do not pre-empt it — let them hit it and fix it.
- **`if` instead of `while` in the shrink step.** Universal. The critique block is the place to catch it.
- **Students apply opposite-end two pointers to unsorted arrays.** The fix is the question "what does the comparison tell you?" — make it a reflex.
- **The amortised O(n) argument does not land the first time.** Repeat it in Week 6 for monotonic stacks; it usually lands on the second exposure.

### Watch list
This is the first week where a student can be fluent in syntax and still stuck, because it demands an *insight* rather than an implementation. A student who is fine here is probably fine through Phase I; a student stuck here needs Mode A intervention (`01-instructor/05-intervention-guide.md`) before Week 6.

### What to cut if you are behind
1. Flex blocks — but **keep LC 424**; it is the deepest idea of the week
2. LC 611 and LC 986 from the weekend set
3. LC 443 from Wednesday's assignment
4. The fast/slow preview on Wednesday — Week 5 teaches it properly anyway

**Never cut:** the three-step window template, the amortised O(n) argument, or the "sortedness is what makes the comparison informative" principle.
