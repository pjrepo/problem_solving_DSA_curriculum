# Week 6 — Stacks & Queues

**Phase I: Foundations** · Difficulty band: **Medium** · Friday: **Checkpoint 1 (graded)**

> The last week of Phase I, and it does double duty. Stacks and monotonic stacks are high-yield interview content in their own right — and the **call stack** introduced here is the concrete mental model that makes Week 7's recursion week survivable. Teach the stack as a physical thing this week and recursion stops being magic next week.

---

## Exit criteria

- [ ] Recognise "match/nest/undo/most-recent-first" as stack triggers
- [ ] Implement a stack that answers `min()` in O(1)
- [ ] Explain what the **call stack** is and what a stack frame contains
- [ ] Write the monotonic stack template and state what invariant the stack maintains
- [ ] Prove the monotonic stack is O(n) using the amortised argument
- [ ] Use a deque for a sliding-window extremum
- [ ] Pass Checkpoint 1 (2 of 3 problems, unlabelled)

---

## Session 1 (Tuesday) — Stack Fundamentals & the Call Stack

### Weekend-set debrief (0:00–0:15)
Debrief **LC 86 Partition List** — the two-dummy-heads technique surprises people and generalises well. Then **LC 92 Reverse Linked List II**, the direct precursor to LC 25 from the contest; ask who saw the connection while the clock was running.

### Concept spine (0:15–0:45)

**Part A — the stack as a physical object.** Do not define LIFO abstractly. Stack books on the desk. *"Which one can I take? Which one did I put down most recently?"*

**The trigger signals** — put these on the board:

| Statement contains | Reach for a stack |
|---|---|
| "matching" / "balanced" / "valid parentheses" | ✓ |
| "nested" structure | ✓ |
| "undo" / "backtrack to the previous" | ✓ |
| "most recent" / "last seen" | ✓ |
| "evaluate an expression" | ✓ |

In Python a plain `list` is a stack: `append` and `pop`, both O(1). **Do not use `insert(0)`/`pop(0)`** — those are O(n) and turn an O(n) algorithm into O(n²). Say why, referring back to Week 1's cost table.

**Part B — LC 20, derived.** `"([{}])"`. Why can't you just count brackets? Because `"([)]"` has balanced counts and is invalid. *"You need to know which bracket is still open most recently. That is a stack."*

**Part C — Min Stack (LC 155).** *"Give me `min()` in O(1) on a stack that changes."*

Let students propose scanning (O(n)) or a single `min` variable (breaks on pop — ask what happens when you pop the minimum). Then the answer: **a second stack that records the minimum at each depth.**

> The general lesson, and it recurs in Week 15's design problems: *"When one structure cannot answer a query fast enough, carry a second structure alongside it."*

**Part D — the call stack.** Spend the last 8 minutes here. This is a deliberate investment in Week 7.

```python
def a(): b()
def b(): c()
def c(): print("here")
```

Draw the frames stacking up and unwinding. State clearly:
- Every function call pushes a **frame**: its local variables and the return address
- Returning pops the frame
- The frame below resumes exactly where it left off
- Too many frames without returning → **stack overflow** (Python: `RecursionError`, default limit ~1000)

> *"Next week we write functions that call themselves. When that feels like magic, come back to this picture — it is just frames on a stack, and each one has its own copy of the variables."*

This paragraph is the highest-leverage thing said in Phase I.

### Live-code (0:45–1:05)
**LC 20 Valid Parentheses** with the closing→opening map, then **LC 155 Min Stack** with the paired-stack approach.

**JS delta:** `arr.push()` / `arr.pop()` behave identically. `arr.shift()` is O(n) — same warning as Python's `pop(0)`.

### Guided practice (1:10–1:50)
1. **LC 20 Valid Parentheses** — Easy
2. **LC 1047 Remove All Adjacent Duplicates In String** — Easy
3. **LC 155 Min Stack** — Medium
4. **LC 150 Evaluate Reverse Polish Notation** — Medium

### Live critique (1:50–2:00)
An *LC 20*. Focus: did they check the stack is empty at the end? `"((("` returns "valid" in a surprising number of submissions. Also: popping from an empty stack on an unmatched closer.

### Flex (2:00–2:30)
**LC 71 Simplify Path** — Medium. A stack applied to something that does not look like a bracket problem, which is exactly the transfer worth practising.

### Common misconceptions
- Forgetting the final "stack must be empty" check.
- Popping an empty stack.
- Using `pop(0)` and destroying the complexity.
- In Min Stack, storing only the global minimum rather than one per depth.

### Assignment 6.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 20 Valid Parentheses | Easy | 20 min |
| LC 682 Baseball Game | Easy | 20 min |
| LC 1047 Remove All Adjacent Duplicates In String | Easy | 20 min |
| LC 155 Min Stack | Medium | 30 min |
| LC 150 Evaluate Reverse Polish Notation | Medium | 30 min |

---

## Session 2 (Wednesday) — Monotonic Stack

> The hardest idea in Phase I, and one of the highest-yield patterns in interviews.

### Warm-up (0:00–0:10)
1. Why is counting brackets insufficient for LC 20?
2. What does a stack frame contain?
3. Why is `list.pop(0)` a bad idea?

### Concept spine (0:10–0:45)

**Start with the brute force.** *Daily Temperatures* (LC 739): for each day, how many days until a warmer one?

```python
for i in range(n):
    for j in range(i+1, n):
        if T[j] > T[i]: ans[i] = j - i; break
```
O(n²). *"What are we doing repeatedly?"* → rescanning the same future days over and over.

**The reframe — and this is the whole insight:**

> *"Instead of asking 'for each day, when does it get warmer?', ask 'for each day, which earlier days was I waiting for?' When today is warmer than the day on top of the stack, today is that day's answer."*

Keep a stack of **indices whose answer is still unknown**, and keep it monotonically decreasing in temperature.

```python
def daily_temperatures(T):
    ans = [0] * len(T)
    stack = []                          # indices, temperatures decreasing
    for i, t in enumerate(T):
        while stack and T[stack[-1]] < t:
            j = stack.pop()             # today resolves day j
            ans[j] = i - j
        stack.append(i)
    return ans
```

**State the invariant explicitly**, and make students say it back:
> *"The stack holds indices, in decreasing order of temperature, of days still waiting for an answer."*

Once they can state the invariant the code writes itself. Without it, they will memorise the shape and fail on any variant.

**The complexity argument — do this properly.** *"There is a `while` inside a `for`. Is it O(n²)?"*

No: **each index is pushed exactly once and popped at most once**, so total work across the whole run is at most 2n. **O(n).**

Then name the connection out loud: *"That is the same argument as the sliding window in Week 3, and as `list.append` in Week 1. Third time you've met it — this is a standard tool for reasoning about loops that look nested but aren't."*

**Increasing vs decreasing:** for the *next greater* element keep a decreasing stack; for the *next smaller*, an increasing one. Have them derive the second from the first rather than memorising both.

### Live-code (0:45–1:05)
**LC 496 Next Greater Element I** (simplest form), then **LC 739 Daily Temperatures**. Then ask what changes for **LC 503 Next Greater Element II** (circular array) — the answer, iterating twice modulo n, should come from the room.

### Guided practice (1:10–1:50)
1. **LC 496 Next Greater Element I** — Easy
2. **LC 739 Daily Temperatures** — Medium
3. **LC 503 Next Greater Element II** — Medium
4. **LC 901 Online Stock Span** — Medium. The same pattern arriving as a stream.

### Live critique (1:50–2:00)
An *LC 739*. Focus: can the author **state the stack invariant**? Ask directly. Working code with an unstatable invariant means it was pattern-matched, and it will not survive a variant.

### Flex (2:00–2:30)
**LC 84 Largest Rectangle in Histogram** — Hard. The monotonic stack's most demanding application. Start it here; it is in the weekend set.

### Common misconceptions
- Storing values rather than indices when the answer needs a distance.
- Wrong comparison direction, producing next-smaller instead of next-greater.
- Believing it is O(n²) because of the nested loop.
- Forgetting to handle indices left on the stack at the end (they have no answer).

### Assignment 6.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 496 Next Greater Element I | Easy | 25 min |
| LC 739 Daily Temperatures | Medium | 30 min |
| LC 503 Next Greater Element II | Medium | 30 min |
| LC 901 Online Stock Span | Medium | 35 min |

---

## Session 3 (Thursday) — Queues, Deques & Design

### Warm-up (0:00–0:10)
1. State the stack invariant for Daily Temperatures.
2. Why is the monotonic stack O(n)?
3. Decreasing stack — next greater or next smaller?

### Concept spine (0:10–0:40)

**Part A — queues.** FIFO. `collections.deque` gives O(1) at both ends; a plain list does not (`pop(0)` is O(n)).

*"Where have you already used a queue without calling it one?"* — nowhere yet, but flag forward: **BFS in Week 11 is a queue and nothing else.** Planting it here means Week 11's BFS costs almost no new concept.

**Part B — the monotonic deque (LC 239).** Sliding window maximum. The window moves; you need the max in O(1).

- A heap gives O(n log n) — mention it, and note that Week 10 covers heaps properly
- A monotonic deque gives **O(n)**

> *"Keep a deque of indices whose values are decreasing. The front is always the window's maximum. Two rules: drop the front if it has slid out of the window, and drop from the back anything smaller than the incoming value — because a smaller earlier element can never be the maximum again while a larger later one exists."*

The second rule is the insight. Make sure they can justify it, not just apply it.

**Part C — design problems.** LC 232 (queue from two stacks) is worth doing properly because of its amortised argument: elements move from the input stack to the output stack **at most once each**, so `pop` is O(1) amortised despite an occasional O(n) transfer. Fourth encounter with amortised analysis — by now it should feel routine.

### Live-code (0:40–1:05)
**LC 239 Sliding Window Maximum** with `deque`, tracing the window over `[1,3,-1,-3,5,3,6,7]` on the board.

### Guided practice (1:10–1:50)
1. **LC 232 Implement Queue using Stacks** — Easy
2. **LC 225 Implement Stack using Queues** — Easy
3. **LC 933 Number of Recent Calls** — Easy. A queue as a sliding time window.
4. **LC 239 Sliding Window Maximum** — Hard

### Live critique (1:50–2:00)
An *LC 232*. Focus: is the author's amortised argument correct? Many will claim O(1) worst case, which is wrong. Precision about *which* kind of complexity claim you are making is itself an interview skill.

### Flex (2:00–2:30) — Checkpoint 1 preparation
Not new content. Put four unlabelled problems from Weeks 1–6 on the projector and ask **only**: *"What pattern is this, and why?"* No coding. This is a direct rehearsal for tomorrow's format, and it is the first time students practise recognition in isolation.

### Common misconceptions
- Using a list with `pop(0)` for a queue.
- In LC 239, storing values instead of indices, so you cannot tell when an element leaves the window.
- Transferring stacks on every operation in LC 232, destroying the amortised bound.

### Assignment 6.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 232 Implement Queue using Stacks | Easy | 25 min |
| LC 225 Implement Stack using Queues | Easy | 25 min |
| LC 933 Number of Recent Calls | Easy | 20 min |
| LC 239 Sliding Window Maximum | Hard | 45 min |

---

## Session 4 (Friday) — ARENA: CHECKPOINT 1 (graded)

**Mode:** Checkpoint · covers Weeks 1–6 · **7% of the final grade**

### Format
- **3 problems, 2 hours, individual**
- **Problems are unlabelled** — no pattern hints. Recognition is being assessed.
- For each problem students must submit: working code, **stated time and space complexity with justification**, and **one sentence naming the pattern and the trigger that identified it**
- Graded against the code review rubric (`01-instructor/03-assessment-and-rubrics.md`, §2), 25 points
- **Passing bar: solve 2 of 3**

### Problems
| # | Problem | Difficulty | Pattern (not shown to students) |
|---|---|---|---|
| 1 | LC 189 Rotate Array | Medium | In-place manipulation / triple reversal |
| 2 | LC 1249 Minimum Remove to Make Valid Parentheses | Medium | Stack |
| 3 | LC 1358 Number of Substrings Containing All Three Characters | Medium | Sliding window |

All three are fresh — none has been assigned. Each maps to a different Phase I pattern, so the results tell you *which* foundation is weak per student, not just that something is.

### After the checkpoint — the most important instructor task of Phase I

Grade within the week and **sort the cohort into three groups** (see `01-instructor/05-intervention-guide.md`, §6):

| Group | Score | Action |
|---|---|---|
| Solid | 18–25 | Proceed; offer stretch problems |
| Shaky | 12–17 | Proceed, with targeted revision in Week 7's flex blocks |
| **At risk** | **below 12** | **One-to-one before Week 7 ends.** Do not let them enter the trees/DP chain with a Phase I hole |

Checkpoint 1 sits immediately before the recursion week for exactly this reason. Its diagnostic value exceeds its assessment value.

### Assignment 6.4 (2h)
| Task | Budget |
|---|---|
| Re-solve any checkpoint problem you did not complete | 45 min |
| **Written:** for each of the three problems, name the pattern, its trigger signal, and — if you missed it — what you saw instead | 30 min |
| Solve **LC 394 Decode String** — Medium, nested stack | 45 min |

The written reflection is the point. A student who can articulate *why* they misread a problem rarely misreads it the same way twice.

---

## Weekend Set 6 (6h) — due Tuesday, Week 7

Slightly lighter than usual: the checkpoint was Friday, and Week 7 is the hardest week of the course.

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 946 Validate Stack Sequences | Medium | 40 min |
| LC 402 Remove K Digits | Medium | 45 min |
| LC 622 Design Circular Queue | Medium | 35 min |
| LC 84 Largest Rectangle in Histogram | Hard | 60 min |
| LC 32 Longest Valid Parentheses | Hard | 60 min |

**LC 402** is a monotonic stack in disguise — the trigger ("remove k to make the smallest number") does not look like one, which is the point. **LC 84** is the week's summit; expect most students to need the editorial, and say so in advance.

### Spaced revision (1h) — from Weeks 5 and 3
| Problem | Source | Target time |
|---|---|---|
| LC 206 Reverse Linked List | Week 5 | **under 5 min** — this must be automatic |
| LC 3 Longest Substring Without Repeating Characters | Week 3 | under 15 min |

### Written editorial (1h)
**LC 84 Largest Rectangle in Histogram.**

The observation to look for: *"For each bar, the largest rectangle with that bar as its height extends left until a shorter bar and right until a shorter bar — so the problem reduces to finding the previous-smaller and next-smaller element for every bar, which is exactly the monotonic stack."*

That reduction is the entire problem. A student who states it has understood monotonic stacks properly; one who describes the pop-and-compute loop without the reduction has memorised a shape.

---

## Instructor notes

### What usually goes wrong this week
- **The monotonic stack does not land on first exposure.** Expect roughly half the class to be shaky Wednesday evening. The invariant question in critique is the diagnostic — if they cannot state it, re-teach on Thursday rather than moving on.
- **Students memorise the Daily Temperatures code** and then fail LC 503 and LC 901. Cure: make them state the invariant before writing anything, every time.
- **Checkpoint anxiety.** Frame it accurately: *"This is diagnostic. It tells me what to teach you next. Two out of three is a pass."*
- **The call stack segment gets cut for time.** Do not let it. It is Week 7's foundation, and eight minutes here saves an hour next week.

### Watch list
Checkpoint 1 is your most informative measurement so far. Combine it with your mock interview notes from Week 4 and your guided-practice observations. **By Monday of Week 7 you should be able to name, for each of the 14 students, their single weakest area.** If you cannot, re-read the checkpoint scripts.

### What to cut if you are behind
1. LC 71 (Tuesday flex)
2. LC 225 and LC 933 from Thursday — LC 232 carries the amortised lesson alone
3. LC 32 and LC 622 from the weekend set
4. LC 682 from Tuesday's assignment

**Never cut:** the call-stack segment, the monotonic stack invariant derivation, the amortised O(n) argument, or Thursday's flex recognition drill.
