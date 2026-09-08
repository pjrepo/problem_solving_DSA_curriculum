# Week 7 — Recursion

**Phase II: Techniques** · Difficulty band: **Medium** · ★ **The critical week of the course**

> Read `01-instructor/05-intervention-guide.md`, §3 Mode E **before** teaching this week.
>
> Recursion is the single largest wall for students who have spent two years writing loops, and it gates everything downstream: trees (Week 9), divide-and-conquer (Week 8), dynamic programming (Weeks 13–14), and backtracking (Week 15). This is why it gets three sessions rather than one lecture, why Week 6 taught the call stack as a physical object, and why Checkpoint 1 was scheduled immediately before it.
>
> **If more than four students have not internalised recursion by Friday, do not proceed to Week 8 on schedule.** Use Week 8's flex blocks for remediation. Falling one week behind is far cheaper than building Weeks 9–15 on a broken foundation.

---

## Exit criteria

- [ ] Trace a recursive call by hand, drawing every stack frame, without an editor
- [ ] Answer the three questions for any recursive function: base case, smaller subproblem, how to combine
- [ ] Write a recursive function for a problem they have never seen
- [ ] Draw a recursion tree and read the complexity off it
- [ ] Explain why naive Fibonacci is O(2ⁿ) and what memoization changes
- [ ] Implement merge sort from scratch and derive its O(n log n)
- [ ] Explain the trade-off between recursive and iterative solutions (stack space)

---

## Session 1 (Tuesday) — The Mental Model

> **No editor for the first hour.** Paper only. This is deliberate and it is the single most effective intervention available for teaching recursion.

### Weekend-set debrief (0:00–0:15)
Debrief **LC 84 Largest Rectangle**. Ask for the reduction — "previous smaller and next smaller for every bar" — rather than the code. Then briefly return Checkpoint 1 results, framed as diagnostic. Individual conversations with the at-risk group happen outside class this week.

### Framing (0:15–0:20) — say this explicitly

> *"This week is the hardest week of the course, and it is hard for almost everybody. If it feels like you have suddenly become bad at programming, that is the normal experience of learning recursion after two years of loops. It passes. Tell me if you are stuck — do not sit on it."*

Pre-empting the wall halves its morale cost.

### Concept spine (0:20–0:55)

**Part A — reconnect to the call stack.** Redraw Week 6's stack-frame picture. Then:

```python
def countdown(n):
    if n == 0:              # base case
        return
    print(n)
    countdown(n - 1)        # a call like any other
```

> *"A function calling itself is not special. It is just another call, pushing another frame. The only new thing is that the frames happen to run the same code — each with its own copy of `n`."*

That last clause is the one to repeat. **Every frame has its own variables.** Most confusion is students imagining a single shared `n`.

**Part B — trace it on paper.** Everyone draws `countdown(3)`, frame by frame, going down and then unwinding. Walk the room and check all 14 diagrams. **Do not proceed until every student's diagram is right.**

Then `factorial(4)`, which returns values rather than printing — the unwinding now carries information back up:

```
factorial(4) → 4 * factorial(3)
                   factorial(3) → 3 * factorial(2)
                                      factorial(2) → 2 * factorial(1)
                                                         factorial(1) → 1   ← base case
                                      factorial(2) = 2 * 1 = 2
                   factorial(3) = 3 * 2 = 6
factorial(4) = 4 * 6 = 24
```

Draw the descent and the return values coming back up. **This diagram is the whole session.**

**Part C — the three questions.** Put these on the board permanently. Every recursive function, forever:

1. **What is the base case?** The smallest input I can answer immediately.
2. **What is the smaller subproblem?** How do I reduce toward the base case?
3. **How do I combine?** Given the answer to the smaller problem, how do I build mine?

**Part D — the leap of faith.** This is the actual block for most students. They cannot write the recursive call because they do not believe it will work.

> *"You do not need to trace it. You need to assume it works. When you write `factorial(n-1)`, assume it returns the correct factorial of n−1 — because you are going to make sure it does."*

**The trick that works:** make them write the assumption as a literal comment.

```python
def reverse(s):
    if len(s) <= 1:
        return s
    # ASSUME reverse(s[1:]) correctly reverses the rest
    return reverse(s[1:]) + s[0]
```

Have every student write that comment on their next five recursive functions. It sounds trivial. It is the difference between "I can't see how this works" and writing the function.

**Part E — the two failure modes.**
- **No base case** → `RecursionError` (Python's default limit is around 1000 frames)
- **A base case never reached** → same, because the input is not actually shrinking

*"If you get a RecursionError, ask: does my input get strictly smaller on every call, and does it eventually hit the base case?"*

### Live-code (0:55–1:15) — editors open now
`factorial`, `sum_list`, `reverse_string`, each written with the three questions answered aloud first, in order. Then show `sys.setrecursionlimit` exists and explain why relying on it is usually a sign the solution should be iterative.

**JS delta:** no tail-call optimisation in practice; default stack depth ~10,000 frames — deeper than Python's, still finite.

### Guided practice (1:20–1:50)
1. **LC 509 Fibonacci Number** — Easy. Naive recursion; do not optimise yet.
2. **LC 344 Reverse String** — Easy, recursively, in place with two indices.
3. **LC 231 Power of Two** — Easy. Recursive halving.
4. **LC 206 Reverse Linked List** — Easy, **recursively**. They know the iterative version cold; the recursive one is a genuinely different way to see it, and it is superb preparation for trees.

LC 206 recursive is the session's payoff. Expect real struggle with `head.next.next = head`; draw it.

### Live critique (1:50–2:00)
The recursive *LC 206*. Focus: can the author explain what `reverse(head.next)` returns and why `head.next` is set to `None`? This is the leap of faith in its purest form.

### Flex (2:00–2:30)
Paper tracing drill, no editors. Trace by hand: `fib(5)` (draw the full tree), `sum_list([1,2,3])`, and a recursive `power(2, 5)`. Collect and check the diagrams — this is your diagnostic for who is at risk.

### Common misconceptions
- Imagining one shared copy of the variables across frames.
- Missing or unreachable base case.
- Forgetting to `return` the recursive call's result.
- Believing recursion is "just a slower loop" — it is a different way of decomposing a problem.
- Trying to trace the whole call tree mentally instead of trusting the recursive call.

### Assignment 7.1 (2h)
| Task | Difficulty | Budget |
|---|---|---|
| **On paper:** trace `factorial(5)`, `fib(4)` (full tree), and `reverse("abcd")`. Photograph and commit. | — | 30 min |
| LC 509 Fibonacci Number | Easy | 15 min |
| LC 344 Reverse String (recursive) | Easy | 20 min |
| LC 231 Power of Two | Easy | 15 min |
| LC 206 Reverse Linked List (recursive) | Easy | 40 min |

**The paper tracing is graded.** It is the highest-signal artefact of the week — you can see exactly who has the model and who does not.

---

## Session 2 (Wednesday) — Recursion Trees & Complexity

### Warm-up (0:00–0:10)
1. State the three questions.
2. What does each stack frame own?
3. `def f(n): return f(n-1)` — what happens, and why?

### Concept spine (0:10–0:45)

**Part A — the recursion tree.** Draw `fib(5)` in full. Let students count the nodes: 15 calls for n=5.

*"How many for `fib(30)`?"* → over 1.3 million. *"For `fib(50)`?"* → about 10¹⁰; you would wait hours.

**Read the complexity off the tree:**
- Each node makes 2 children → the tree roughly doubles per level
- Depth is n
- Nodes ≈ 2ⁿ → **O(2ⁿ)**

**Part B — the waste.** Circle `fib(3)` everywhere it appears in the tree. It is computed repeatedly, from scratch, each time.

*"What if we just remembered?"*

```python
def fib(n, memo={}):
    if n <= 1: return n
    if n in memo: return memo[n]
    memo[n] = fib(n-1, memo) + fib(n-2, memo)
    return memo[n]
```

O(2ⁿ) → **O(n)**, with three added lines.

**Then stop, and say this precisely:**

> *"That is dynamic programming. You have now written one. In Week 13 we will do this systematically — the hard part there is not the memo dictionary, it is defining precisely what the state means. Notice that here it was easy because `fib(n)` obviously means 'the nth Fibonacci number.'"*

Planting the *state definition* idea six weeks early is one of the highest-value things in this curriculum. **Do not skip it.**

(Mention `functools.lru_cache` exists, then set it aside — students should write the memo dictionary by hand until Week 13.)

**Part C — reading complexity from the recursion shape.**

| Shape | Complexity | Example |
|---|---|---|
| One call, n−1 | O(n) | factorial |
| One call, n/2 | O(log n) | binary search, fast power |
| Two calls, n−1 | O(2ⁿ) | naive fib |
| Two calls, n/2, O(n) merge | O(n log n) | merge sort |
| Two calls, n/2, O(1) work | O(n) | tree traversal |

*"Ask two questions: how many calls does each level make, and how much does the input shrink? That determines everything."*

**Part D — space.** Recursion costs stack space proportional to the **maximum depth**, not the number of calls. Naive `fib(n)` makes 2ⁿ calls but uses only O(n) stack, because the tree is explored depth-first. This distinction is regularly asked and regularly missed.

### Live-code (0:45–1:05)
**LC 50 Pow(x, n)** — fast exponentiation. Derive it: *"To compute x²⁰, do I need 20 multiplications?"* → `x²⁰ = (x¹⁰)²`. O(log n).

Handle the traps explicitly: negative `n`, and `n = 0`.

### Guided practice (1:10–1:50)
1. **LC 50 Pow(x, n)** — Medium
2. **LC 21 Merge Two Sorted Lists** — Easy, **recursively**. They wrote it iteratively in Week 5; the recursive version is four lines and makes the structure obvious.
3. **LC 24 Swap Nodes in Pairs** — Medium, recursively. From Weekend 5, now revisited with the right tool.
4. **LC 779 K-th Symbol in Grammar** — Medium. Pure recursive reasoning with no data structure; excellent for forcing the three questions.

### Live critique (1:50–2:00)
The recursive *LC 21*. Compare against the Week 5 iterative version on the projector. Ask which they would write in an interview. **There is no single right answer** — recursive is shorter and clearer, iterative uses O(1) stack — and the ability to discuss that trade-off is exactly what is being assessed in real rounds.

### Flex (2:00–2:30)
**LC 394 Decode String** — recursively. They solved it with an explicit stack in Week 6's assignment. Solving it again recursively and comparing makes the point that **recursion and an explicit stack are the same thing**, one managed by the language and one by you. That equivalence is worth 30 minutes.

### Common misconceptions
- Believing memoization changes the *algorithm* rather than eliminating repeated work.
- A mutable default argument (`memo={}`) shared across calls — demonstrate the bug, then show the `None` idiom.
- Confusing number of calls with stack depth.
- In LC 50, forgetting negative exponents or overflow-adjacent edge cases.

### Assignment 7.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 50 Pow(x, n) | Medium | 30 min |
| LC 21 Merge Two Sorted Lists (recursive) | Easy | 20 min |
| LC 24 Swap Nodes in Pairs (recursive) | Medium | 30 min |
| LC 779 K-th Symbol in Grammar | Medium | 35 min |
| **Written:** draw the recursion tree for `fib(5)`; state how many calls, the time complexity, and the space complexity | — | 15 min |

---

## Session 3 (Thursday) — Divide & Conquer

### Warm-up (0:00–0:10)
1. Why is naive Fibonacci O(2ⁿ)?
2. How many calls does `fib(20)` make? How much stack does it use?
3. `x²⁰` in how few multiplications?

### Concept spine (0:10–0:45)

**Part A — the shape.**
1. **Divide** the problem into independent subproblems
2. **Conquer** each recursively
3. **Combine** the results

*"Recursion reduces to a smaller problem. Divide and conquer splits into several, and the interesting work is usually in the combine step."*

**Part B — merge sort, derived.**

*"Suppose two people each sorted half your array. How would you finish the job?"* → merge two sorted lists, which they have written twice (Week 5 and Wednesday).

```python
def merge_sort(a):
    if len(a) <= 1: return a               # base case
    mid = len(a) // 2
    left  = merge_sort(a[:mid])            # assume it returns sorted
    right = merge_sort(a[mid:])            # assume it returns sorted
    return merge(left, right)              # the real work
```

**The complexity, derived visually rather than by the Master theorem:**
- Each level does O(n) total work in the merges
- There are log n levels (halving until size 1)
- **O(n log n)**

Draw the tree with "n work" written at every level and count the levels. That picture is far more durable than a formula.

**Space:** O(n) for the auxiliary arrays, plus O(log n) stack.

**Part C — quicksort's partition, and quickselect.**

Partition around a pivot: everything smaller left, larger right. *"Where does the pivot end up?"* → its final sorted position.

**Then the insight that matters for interviews:** if you only want the k-th smallest, you do not need to sort both sides — recurse into the one containing k. That is **quickselect**, O(n) average.

Compare honestly:

| | Merge sort | Quicksort |
|---|---|---|
| Time | O(n log n) always | O(n log n) average, **O(n²) worst** |
| Space | O(n) | O(log n) |
| Stable | Yes | No |
| Good on linked lists | **Yes** | No |

*"This is why LC 148 Sort List — which you will write in forty minutes — uses merge sort: splitting a linked list is cheap with fast/slow, and merging needs no auxiliary array, just relinked pointers."*

### Live-code (0:45–1:10)
**Merge sort from scratch**, both `merge_sort` and `merge`. Then the partition function, and quickselect built on it.

### Guided practice (1:15–1:50)
1. **LC 912 Sort an Array** — Medium. Implement merge sort; Python's `sort()` is banned for this one.
2. **LC 215 Kth Largest Element in an Array** — Medium. Quickselect. (A heap also solves it — flag that Week 10 returns to this problem with a different tool.)
3. **LC 148 Sort List** — Medium. Merge sort on a linked list; the fast/slow split from Week 5 does the divide step.

### Live critique (1:50–2:00)
An *LC 912*. Focus: is the merge step correct when one side is exhausted? And is the base case `len <= 1`? Off-by-one in the base case causes infinite recursion, which is a good, visible failure.

### Flex (2:00–2:30)
**LC 23 Merge k Sorted Lists** — Hard, by divide and conquer: merge pairs of lists, then pairs of results. O(N log k). Note that a heap gives the same bound differently, and that Week 10 will show it.

### Common misconceptions
- Base case `len(a) == 0`, so `[x]` recurses forever.
- Merging in place without an auxiliary array, and getting it wrong.
- Claiming quicksort is always O(n log n).
- In quickselect, recursing into both halves — that is just quicksort again.

### Assignment 7.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 912 Sort an Array (merge sort, implemented) | Medium | 45 min |
| LC 215 Kth Largest Element (quickselect) | Medium | 40 min |
| LC 148 Sort List | Medium | 35 min |

---

## Session 4 (Friday) — ARENA: Contest 4

**Mode:** Contest · 90 minutes · recursion-heavy

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 1979 Find Greatest Common Divisor of Array | Easy | Euclid — recursion in its simplest form |
| P2 | LC 1863 Sum of All Subset XOR Totals | Easy | Recursive enumeration; previews backtracking |
| P3 | LC 241 Different Ways to Add Parentheses | Medium | Divide and conquer on expressions |
| P4 | LC 315 Count of Smaller Numbers After Self | Hard | Counting during the merge step |

**Calibration:** P1 and P2 should be reachable by everyone who has the model. **If a student scores zero here, that is an immediate Mode E intervention** — this contest is a direct measurement of whether recursion landed.

### Reveal (1:45–2:00)
Explain **P3** as the archetype: *"Every operator is a possible split point. Solve the left side, solve the right side, combine every pair of results. That is divide and conquer with no array in sight."*

Note that P4 is merge sort with a counter added during the merge — the same structural idea as P3, and worth showing even though few will have solved it.

### Assignment 7.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 241 Different Ways to Add Parentheses** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 7 (6h) — due Tuesday, Week 8

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 397 Integer Replacement | Medium | 35 min |
| LC 486 Predict the Winner | Medium | 45 min |
| LC 698 Partition to K Equal Sum Subsets | Medium | 50 min |
| LC 23 Merge k Sorted Lists | Hard | 50 min |
| LC 493 Reverse Pairs | Hard | 55 min |

**LC 486** is the most important problem here: the recursion only works once you define the return value precisely as *"the maximum score difference the current player can achieve from this state."* That is the state-definition discipline Week 13 is built on. **LC 698** needs recursion plus pruning and previews Week 15. **LC 493** is LC 315's cousin — counting inside the merge.

### Spaced revision (1h) — first full three-way spacing (Weeks 6, 4, 1)
| Problem | Source | Target time |
|---|---|---|
| LC 739 Daily Temperatures | Week 6 (N−1) | under 15 min |
| LC 1 Two Sum | Week 1 (N−6) | **under 5 min** |

LC 1 is here to demonstrate the point of spacing: it should now be automatic. If it is not, the whole retention system needs attention for that student.

### Written editorial (1h)
**LC 486 Predict the Winner.**

The observation to look for: *"Define the recursion to return the score **difference** from the current player's perspective, not either player's absolute score. Then the opponent's optimal play is just the same function with the sign flipped — one function handles both players."*

That reframing — choosing what the function *returns* so the recursion closes cleanly — is precisely the skill Week 13 requires. **This editorial is your best single predictor of who will find DP hard.** Read all 14 carefully and note the names.

---

## Instructor notes

### What usually goes wrong this week — and what to do

| Symptom | Response |
|---|---|
| "I can read it but I can't write it" | The leap of faith. Enforce the `# ASSUME this works` comment for two weeks. |
| Traces the whole tree mentally and gets lost | Forbid it. One level only: base case, one step, combine. |
| Writes the recursive call but forgets to `return` it | Extremely common. Make it a critique focus. |
| Cannot find the base case | Ask "what is the smallest input, and what is its answer?" — never "what is the base case?" |
| Understands `factorial` but not `fib` | The gap is *two* recursive calls. Go back to the recursion tree drawing. |
| Panic and disengagement | Address it directly and privately. Repeat that this week is hard for almost everyone. |

### The Friday decision

After Contest 4, count how many students solved P1 **and** P2.

- **12+ of 14:** proceed to Week 8 as planned.
- **9–11:** proceed, but use **all three of Week 8's flex blocks** for recursion remediation.
- **Fewer than 9:** **do not proceed on schedule.** Re-teach Tuesday's paper-tracing session at the start of Week 8 and let binary search slip by two sessions. Weeks 9, 13, 14 and 15 all sit on this foundation — the week is worth spending.

### What to cut if you are behind
1. Thursday's flex (LC 23) — it reappears in the weekend set
2. LC 779 from Wednesday
3. LC 493 and LC 698 from the weekend set
4. Quickselect from Thursday — keep merge sort, which carries the divide-and-conquer lesson alone

**Never cut:** Tuesday's paper-only first hour, the three questions, the leap-of-faith comment device, the `fib` recursion tree, or the memoization reveal that plants Week 13.
