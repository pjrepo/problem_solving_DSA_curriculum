# Week 6 — Stacks & Queues · Optional Practice

**Phase I** · Week band: **Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> **Friday is Checkpoint 1 — graded.** Do not spend Thursday night on this file. If you have spare hours this week, put them into re-solving Weeks 1–5 material, which is what the checkpoint actually covers.

---

## Session 1 (Tuesday) — Stack Fundamentals & the Call Stack

> Optional. Not graded. Skip freely.

### Check your understanding
1. A stack is the right structure when the problem has what property? Answer in one sentence, not with an example.
2. Valid Parentheses with three bracket types: what do you push, and what do you compare on a closing bracket?
3. Week 7 is recursion. What does the call stack hold for each active call, and why does that make deep recursion a space cost?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1021 Remove Outermost Parentheses | Easy | 20 min |
| LC 1614 Maximum Nesting Depth of the Parentheses | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 921 Minimum Add to Make Parentheses Valid | Medium | 40 min |
| LC 726 Number of Atoms | **Hard** | 55 min |

LC 726 is a parser — nested structure, counts, and a stack of dictionaries. It is the best argument this week for why the stack is a *general* tool and not a parentheses trick.

### 3. Revision — Week 5 (Linked Lists) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 541 Reverse String II | Easy | 25 min |
| LC 445 Add Two Numbers II | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why a stack and not a counter?"** — A good answer says a counter suffices only when there is one bracket type; with several you need to know *which* bracket is innermost, and only a stack preserves that. Knowing when the cheap version is enough is the real skill.
2. **"What is the space complexity in the worst case?"** — A good answer gives O(n) and names the input that causes it — all opening brackets. Quoting O(1) here is the standard mistake.

---

## Session 2 (Wednesday) — Monotonic Stack

> Optional. Not graded. Skip freely.

### Check your understanding
1. State the monotonic stack invariant: at any moment, what is true of the elements in the stack?
2. "Next greater element" — do you push indices or values, and why does the answer usually have to be indices?
3. Each element is pushed once and popped at most once. Write the one-sentence argument that this makes the whole scan O(n).

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1475 Final Prices With a Special Discount in a Shop | Easy | 20 min |
| LC 1544 Make The String Great | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2390 Removing Stars From a String | Medium | 40 min |
| LC 1944 Number of Visible People in a Queue | **Hard** | 55 min |

LC 1944 is the monotonic stack with the answer read off the stack *size* rather than its contents — a genuinely different use of the same structure.

### 3. Revision — Week 5 (Linked Lists) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2108 Find First Palindromic String in the Array | Easy | 25 min |
| LC 581 Shortest Unsorted Continuous Subarray | Medium | 35 min |

### 4. Interview follow-ups
1. **"Prove this is O(n) and not O(n²)."** — A good answer counts total pushes and pops rather than looking at the nested `while`: n pushes, at most n pops, so at most 2n stack operations overall. This is the amortised argument from Week 1, and it is asked constantly.
2. **"What if you needed the next *smaller* element instead?"** — A good answer flips the comparison and says nothing else changes. Being able to state that the structure is the same and only the predicate moves is what makes this a pattern rather than four memorised problems.

---

## Session 3 (Thursday) — Queues, Deques & Design

> Optional. Not graded. Skip freely.

### Check your understanding
1. Queue versus deque: name one operation a deque gives you that a queue does not, and a problem that needs it.
2. Sliding-window maximum with a deque — what do you store, and why do you evict from the *back* as well as the front?
3. You are asked to design a structure with O(1) `push`, `pop` and `getMin`. What is the extra state, and why does it not break the O(1)?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1700 Number of Students Unable to Eat Lunch | Easy | 20 min |
| LC 2073 Time Needed to Buy Tickets | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 384 Shuffle an Array | Medium | 40 min |
| LC 2444 Count Subarrays With Fixed Bounds | **Hard** | 55 min |

### 3. Revision — Week 5 (Linked Lists) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2000 Reverse Prefix of Word | Easy | 25 min |
| LC 1472 Design Browser History | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why a deque rather than a heap for the sliding-window maximum?"** — A good answer compares O(n) with O(n log n) and explains that the deque works because elements leave the window in the same order they entered — a heap cannot exploit that.
2. **"How would you make this thread-safe?"** — A good answer says what needs protecting and admits if concurrency is outside what it knows. Saying "I have not done concurrent programming, but the invariant that needs protecting is X" scores better than bluffing.

---

## Session 4 (Friday) — ARENA: CHECKPOINT 1 (graded)

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Checkpoint day. Problems are **unlabelled**. For each of these signals, name the structure: "next greater", "matching pairs", "most recent first", "fixed-size window maximum".
2. Which Phase I week do you feel least solid on? Be honest — that is where the checkpoint will find you.
3. From memory, in a blank file: the monotonic stack template. Then the min-stack. Time both.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1598 Crawler Log Folder | Easy | 20 min |
| LC 3174 Clear Digits | Easy | 25 min |

Use these as a five-minute warm-up before the checkpoint if you want, not as revision. Actual checkpoint preparation is re-solving Weeks 1–5, not new problems.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 853 Car Fleet | Medium | 40 min |
| LC 2197 Replace Non-Coprime Numbers in Array | **Hard** | 55 min |

### 3. Revision — Week 5 (Linked Lists) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 696 Count Binary Substrings | Easy | 25 min |
| LC 1877 Minimize Maximum Pair Sum in Array | Medium | 35 min |

### 4. Interview follow-ups
1. **"You were not told which pattern to use. How did you decide?"** — A good answer names the trigger phrase in the problem statement that pointed at the structure. Pattern recognition is the skill Checkpoint 1 assesses, and being able to say *why* you reached for something is the evidence that you have it.
2. **"Your solution works. Would you ship it?"** — A good answer separates correctness from quality: naming, edge cases, and whether a stranger could read it in 30 seconds. From this checkpoint on, the code review rubric is part of your grade, not an extra.
