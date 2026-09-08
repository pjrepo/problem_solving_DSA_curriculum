# Week 4 — Strings & Matrices

**Phase I: Foundations** · Difficulty band: **Medium** · Friday: **first mock interview**

> Strings are arrays with extra rules; matrices are arrays with two indices. Neither is conceptually new after Week 3 — which is exactly why this is the right week for the first mock interview. Students have enough material to be interviewed on, and the difficulty is in *articulation*, not new theory.

---

## Exit criteria

- [ ] Explain why string concatenation in a loop is O(n²) and use `join` instead
- [ ] Write expand-around-centre for palindromes and explain the 2n−1 centres
- [ ] Apply the sliding-window template to strings, including the **minimising** inversion
- [ ] Rotate a matrix in place via transpose + reverse, and explain why it works
- [ ] Traverse a matrix in spiral order with correct boundary handling
- [ ] Complete a 45-minute mock interview: clarify, state the approach, code while narrating, test

---

## Session 1 (Tuesday) — Strings & Palindromes

### Weekend-set debrief (0:00–0:15)
Debrief **LC 18 4Sum** — check whether students generalised the 3Sum structure (anchor two, two-pointer the rest) or wrote something bespoke. Then **LC 567 / LC 438**: ask who noticed they were the same problem. That question, asked repeatedly, is how pattern-thinking gets built.

### Concept spine (0:15–0:45)

**Part A — strings are immutable, and it costs you.** Revisit Week 1's demonstration with the reason attached: every `s += x` allocates a new string and copies the old one, so building n characters costs 1+2+3+…+n = **O(n²)**.

```python
parts = []
for w in words: parts.append(w)     # O(1) each
result = "".join(parts)             # O(n) once
```

**In JavaScript** strings are also immutable, but engines optimise `+=` with ropes — so it is often fine in practice, and `arr.join('')` is still the right habit. Say both things; students will hit the difference.

**Part B — palindromes and expand-around-centre.**

Brute force for the longest palindromic substring: every substring, check each → O(n³).

*"What is the defining property of a palindrome?"* → symmetry about a centre.

*"So instead of checking every substring, what if we grew outward from every possible centre?"*

**The subtlety students miss: there are 2n−1 centres**, not n — a palindrome can be centred on a character (odd length) or between two characters (even length). Draw both on the board.

```python
def longest_palindrome(s):
    def expand(l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1; r += 1
        return s[l+1:r]                 # note the +1 / no-op: we overshot by one

    best = ""
    for i in range(len(s)):
        for cand in (expand(i, i), expand(i, i+1)):   # odd centre, even centre
            if len(cand) > len(best): best = cand
    return best
```

O(n²) time, O(1) extra space. **Mention that an O(n) algorithm exists (Manacher's) and that we are not covering it** — it is a specialist tool that essentially never appears in interviews, and saying so explicitly teaches good judgement about what to learn.

### Live-code (0:45–1:05)
**LC 5 Longest Palindromic Substring** with expand-around-centre. Then **LC 647 Palindromic Substrings** — the identical helper, counting instead of measuring. Two problems, one function: a clean illustration of a reusable core.

### Guided practice (1:10–1:50)
1. **LC 14 Longest Common Prefix** — Easy. Vertical scanning.
2. **LC 9 Palindrome Number** — Easy. Without converting to a string.
3. **LC 5 Longest Palindromic Substring** — Medium
4. **LC 647 Palindromic Substrings** — Medium

### Live critique (1:50–2:00)
An *LC 5*. Focus: did they handle even-length centres? Solutions that pass the samples but fail on `"abba"` are common and make the point vividly.

### Flex (2:00–2:30) — awareness-level topic
**LC 28 Find the Index of the First Occurrence in a String.** Naive matching is O(n·m). Explain, without implementing:

- **Rabin–Karp** — hash the pattern, roll the hash across the text, compare hashes and verify on a match. Average O(n+m).
- **KMP** — precompute a failure table so that on a mismatch you never re-examine text you have already matched. Worst case O(n+m).

**Tell students explicitly:** *"You should be able to say those two paragraphs in an interview. You will almost certainly never be asked to implement either. Knowing which algorithms are worth memorising is itself a skill."*

### Common misconceptions
- Only checking odd-length palindrome centres.
- Off-by-one in the return slice after the expand loop overshoots.
- Building strings with `+=` in a loop.
- Assuming `s[i]` is O(1) — it is, but slicing `s[i:j]` is O(j−i), which hides cost inside loops.

### Assignment 4.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 14 Longest Common Prefix | Easy | 20 min |
| LC 151 Reverse Words in a String | Medium | 25 min |
| LC 5 Longest Palindromic Substring | Medium | 35 min |
| LC 647 Palindromic Substrings | Medium | 25 min |
| LC 28 Find the Index of the First Occurrence | Easy | 20 min |

---

## Session 2 (Wednesday) — Sliding Window on Strings

> Week 3's template, applied where it is hardest. **LC 76 is the payoff problem of Phase I.**

### Warm-up (0:00–0:10)
1. How many palindrome centres are there in a string of length n?
2. Why is `s += x` in a loop O(n²)?
3. State the three steps of the sliding-window template.

### Concept spine (0:10–0:40)

**Recap the template in 3 minutes** (expand, shrink-while-invalid, record), then introduce the inversion.

**The two shapes of variable window:**

| Goal | Shrink while | Record |
|---|---|---|
| **Longest** valid window | window is **invalid** | after shrinking |
| **Shortest** valid window | window is **valid** | *while* shrinking, before it breaks |

*"Everything you did last week was the first row. Today is the second, and it flips the loop."*

**LC 76 Minimum Window Substring.** Find the shortest substring of `s` containing all characters of `t` (with multiplicity).

The hard part is not the window — it is **tracking validity in O(1) per step.** Naively comparing two Counters at every step is O(k) and ruins the complexity.

The device:

```python
def min_window(s, t):
    need = Counter(t)
    missing = len(t)                 # total characters still needed (with multiplicity)
    left = 0
    best = (float('inf'), 0, 0)

    for right, ch in enumerate(s):
        if need[ch] > 0: missing -= 1     # only count it if we actually needed it
        need[ch] -= 1                     # may go negative: a surplus

        while missing == 0:               # window is VALID -> try to shrink
            if right - left + 1 < best[0]:
                best = (right - left + 1, left, right)
            need[s[left]] += 1
            if need[s[left]] > 0: missing += 1   # we just broke validity
            left += 1

    return "" if best[0] == float('inf') else s[best[1]:best[2]+1]
```

**Dwell on two lines.** `need[ch]` going negative encodes surplus, which is what makes the re-check on removal correct. And `missing` is a single integer standing in for a whole-dictionary comparison — **this trick, "maintain a scalar summary of a complex condition," recurs constantly** and is worth naming.

### Live-code (0:40–1:05)
LC 76 in full, slowly, tracing `s = "ADOBECODEBANC"`, `t = "ABC"` on the board alongside the code. This is a 25-minute derivation and it is worth every minute.

### Guided practice (1:10–1:50)
1. **LC 424 Longest Repeating Character Replacement** — Medium. From Week 3's flex; now everyone does it.
2. **LC 904 Fruit Into Baskets** — Medium. "At most 2 distinct" — a clean, gentler variable window.
3. **LC 1493 Longest Subarray of 1's After Deleting One Element** — Medium
4. **LC 76 Minimum Window Substring** — Hard

### Live critique (1:50–2:00)
An *LC 424*. Focus: did they recompute the max frequency inside the loop (O(26n), acceptable) or maintain it (O(n))? A good conversation about when a constant factor is worth optimising and when it is noise.

### Flex (2:00–2:30)
**LC 30 Substring with Concatenation of All Words** — Hard. Stretch for the top of the room; genuinely difficult.

### Common misconceptions
- Comparing full Counters each step, then claiming O(n).
- In the minimising shape, recording the answer after breaking validity instead of before.
- Not letting counts go negative in LC 76, which breaks the restore logic.
- Forgetting the "no valid window" case and returning garbage instead of `""`.

### Assignment 4.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 904 Fruit Into Baskets | Medium | 30 min |
| LC 1493 Longest Subarray of 1's After Deleting One Element | Medium | 30 min |
| LC 76 Minimum Window Substring | Hard | 60 min |

Only three problems — LC 76 deserves an unhurried hour.

---

## Session 3 (Thursday) — Matrices & Grid Thinking

### Warm-up (0:00–0:10)
1. In LC 76, what does `missing` count, and why is it better than comparing dictionaries?
2. When minimising a window, do you shrink while valid or while invalid?
3. `grid[r][c]` — which index is the row?

### Concept spine (0:10–0:40)

**Part A — indexing discipline.** Most matrix bugs are `grid[c][r]`. Establish the convention on the board and never deviate: `rows = len(grid)`, `cols = len(grid[0])`, iterate `for r in range(rows): for c in range(cols):`.

**Watch the initialisation trap:**
```python
grid = [[0] * cols] * rows      # WRONG — every row is the same list object
grid = [[0] * cols for _ in range(rows)]   # right
```
Demonstrate the bug live. It costs students hours if they meet it alone.

**Part B — rotation as composition.** LC 48: rotate 90° clockwise, in place.

Do not give the answer. Put a 3×3 matrix on the board with numbered cells and ask where each element goes. Then reveal:

> **transpose, then reverse each row.**

Show it on the board with actual numbers. It looks like magic until you see it, and then it is obvious — which makes it a genuinely satisfying derivation to run.

**Part C — spiral traversal (LC 54).** Four boundaries — `top`, `bottom`, `left`, `right` — shrinking inward. The trap is the **single remaining row or column** at the end: without a boundary check before the third and fourth passes, elements get emitted twice. Trace a 3×4 matrix by hand.

**Part D — a grid is a graph.** Plant this deliberately:

> *"Every cell has up to four neighbours: up, down, left, right. That makes a grid a graph in disguise. In Week 11 we will search grids with BFS and DFS and it will feel like nothing new."*

Write the direction-vector idiom now, so it is familiar later:
```python
DIRS = [(-1,0), (1,0), (0,-1), (0,1)]
for dr, dc in DIRS:
    nr, nc = r + dr, c + dc
    if 0 <= nr < rows and 0 <= nc < cols:
        ...
```

### Live-code (0:40–1:05)
**LC 48 Rotate Image** (transpose + reverse), then **LC 54 Spiral Matrix** with the boundary checks made explicit.

### Guided practice (1:10–1:50)
1. **LC 867 Transpose Matrix** — Easy. The building block.
2. **LC 48 Rotate Image** — Medium
3. **LC 54 Spiral Matrix** — Medium
4. **LC 73 Set Matrix Zeroes** — Medium. The O(1)-space version uses the first row and column as marker storage — a nice echo of Week 1's "use the array as a hash map".

### Live critique (1:50–2:00)
An *LC 73*. Compare the O(m+n)-space and O(1)-space solutions. Ask which they would present in an interview — and note that stating "here's the simple version, and here's how I'd remove the extra space" is a strong interview move in itself.

### Flex (2:00–2:30)
**LC 289 Game of Life** — Medium. In-place with encoded intermediate states (e.g. 2 = "was 1, will be 0"). Another instance of encoding extra information in existing storage.

### Common misconceptions
- `[[0]*c]*r` aliasing.
- Swapping row/column order in transpose loops — iterate only the upper triangle (`for c in range(r, cols)`), or you transpose twice and get the original back.
- Spiral traversal double-emitting the last row or column.
- Assuming the matrix is square. Check `len(grid) == len(grid[0])` before using square-only tricks.

### Assignment 4.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 867 Transpose Matrix | Easy | 15 min |
| LC 48 Rotate Image | Medium | 30 min |
| LC 54 Spiral Matrix | Medium | 35 min |
| LC 73 Set Matrix Zeroes | Medium | 35 min |

---

## Session 4 (Friday) — ARENA: Mock Interview Round 1

**Mode:** Peer mock interviews · full protocol in `01-instructor/02-arena-playbook.md`, Part B

### Framing (0:00–0:10)

Say this plainly:

> *"Today's problems are easier than what you did on Wednesday. That is deliberate. We are not testing whether you can solve them — we are testing whether you can be watched while you solve them. Expect to score badly. Everyone does in the first round; that is what the first round is for."*

Restate the rubric (`01-instructor/03-assessment-and-rubrics.md`, §3), emphasising that **only 10 of 30 points are for the code.**

### Format
7 pairs · two 45-minute rounds · every student interviews once and is interviewed once. Pair across ability. Rotate continuously, ~6 minutes per pair.

### Problem cards (all Easy, drawn from Weeks 1–3)
| # | Problem | Follow-up to ask if time remains |
|---|---|---|
| 1 | LC 121 Best Time to Buy and Sell Stock | "What if you could make multiple transactions?" |
| 2 | LC 242 Valid Anagram | "What if the strings contain Unicode?" |
| 3 | LC 217 Contains Duplicate | "What if the array is sorted? Can you use O(1) space?" |
| 4 | LC 167 Two Sum II | "What if the array were *not* sorted?" |
| 5 | LC 977 Squares of a Sorted Array | "Can you do it without sorting?" |
| 6 | LC 26 Remove Duplicates from Sorted Array | "What if each element may appear twice?" |
| 7 | LC 283 Move Zeroes | "Minimise the number of writes." |

Each card also carries: two clarifying questions the interviewer should **wait to be asked**, the optimal approach and complexity, and one hint to release only after 15 minutes of no progress.

### Group debrief (1:50–2:00)
Do not review the problems — review the **behaviours**. Use the table in the Arena playbook. The near-universal Round 1 findings:

- Started coding within 30 seconds
- Never asked a single clarifying question
- Went silent for minutes at a time
- Said "it's O(n)" with no justification
- Declared "done" without testing anything

Name these without naming individuals. Tell them Round 2 is in Week 9 and the bar rises to 20/30.

### Assignment 4.4 (2h)
| Task | Budget |
|---|---|
| **Self-review:** re-solve the problem you were given, out loud, recording yourself. Watch it back. | 45 min |
| **Written reflection:** three things you did poorly as a *candidate* and three as an *interviewer*, each with what you will do differently in Week 9 | 30 min |
| Solve **LC 36 Valid Sudoku** — Medium, matrix + hash sets | 45 min |

The recording is the assignment. Students routinely discover they were silent for four minutes and had no idea.

---

## Weekend Set 4 (6h) — due Tuesday, Week 5

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 59 Spiral Matrix II | Medium | 35 min |
| LC 289 Game of Life | Medium | 40 min |
| LC 763 Partition Labels | Medium | 40 min |
| LC 179 Largest Number | Medium | 40 min |
| LC 6 Zigzag Conversion | Medium | 35 min |
| LC 395 Longest Substring with At Least K Repeating Characters | Medium | 50 min |

**LC 179** requires a custom comparator (sort by whether `a+b > b+a`) and previews Week 8. **LC 763** is a greedy/hash hybrid previewing Week 12. **LC 395** does not yield to the standard window template — it needs divide-and-conquer or a window run for each possible distinct-character count. Tell students that explicitly: *"Part of the lesson is recognising when your favourite template does not apply."*

### Spaced revision (1h) — from Weeks 3 and 1
| Problem | Source | Target time |
|---|---|---|
| LC 15 3Sum | Week 3 | under 25 min |
| LC 125 Valid Palindrome | Week 1 | under 10 min |

Deliberate pairing: LC 125 and Week 3's LC 680 are the same shape with one branch added. Ask students to note the relationship in their tracker.

### Written editorial (1h)
**LC 76 Minimum Window Substring.**

The observation to look for: *"Track a single counter of how many required characters are still missing, so validity is an O(1) check instead of a dictionary comparison — and let counts go negative so surplus characters are handled automatically on removal."* If a student writes that, they have understood the problem. If they narrate the code line by line, they have not.

---

## Instructor notes

### What usually goes wrong this week
- **LC 76 takes longer than planned.** Protect the full 25-minute derivation; cut the flex block instead.
- **Mock Round 1 scores are low and morale dips.** Pre-empt it in the framing. Say the number out loud: *"15 out of 30 is a pass today."*
- **The `[[0]*c]*r` aliasing bug** will hit someone. Demonstrating it live is much cheaper than letting them find it at 11pm.
- **Students treat matrix problems as a separate universe.** Keep repeating "a grid is a graph" — it pays off in Week 11.

### Watch list
The mock reveals things assignments cannot: who freezes, who cannot explain working code, who never asks questions. **Take notes per student during the rotation** — these are the most informative 90 minutes of Phase I, and they should feed directly into your Week 6 checkpoint analysis.

### What to cut if you are behind
1. Wednesday's flex (LC 30) and Tuesday's flex (LC 28 / string-matching awareness)
2. LC 6 and LC 395 from the weekend set
3. LC 9 and LC 647 from Tuesday
4. Thursday's flex (LC 289) — but then keep it in the weekend set

**Never cut:** the LC 76 derivation, the mock interview, or the "a grid is a graph" framing.
