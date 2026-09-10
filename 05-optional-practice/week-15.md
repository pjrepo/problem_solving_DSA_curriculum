# Week 15 — Backtracking · Bits · Design · Optional Practice

**Phase III** · Week band: **Medium/Hard** · Reinforce = Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Three loosely related topics, all of which show up in interviews more than their share of teaching time suggests. Backtracking is recursion with an undo; bit manipulation is a small bag of tricks worth having; design is the round that most people have never practised.
>
> **Friday is Mock Round 3 — instructor-conducted, full FAANG format**, scheduled across the week. Treat the Interview track here as the actual preparation.

---

## Session 1 (Tuesday) — Backtracking

> Optional. Not graded. Skip freely.

### Check your understanding
1. Backtracking is recursion plus what one extra step? Name it and say where it goes in the code.
2. Generating subsets versus permutations: what differs in the recursive call, and what does each need to track?
3. Duplicates in the input. Where do you handle them, and why does sorting first make it possible?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 77 Combinations | Medium | 35 min |
| LC 797 All Paths From Source to Target | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 93 Restore IP Addresses | Medium | 40 min |
| LC 37 Sudoku Solver | **Hard** | 55 min |

### 3. Revision — Week 14 (DP II) · Week 12 (Graphs II & Greedy) · Week 9 (Trees I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 67 Add Binary | Easy | 25 min |
| LC 222 Count Complete Tree Nodes | Medium | 35 min |

### 4. Interview follow-ups
1. **"What is the complexity of generating all subsets?"** — A good answer gives O(n · 2ⁿ) — 2ⁿ subsets, each costing O(n) to copy — and notes that the copy is the factor candidates forget. Getting the copy cost right is the detail that lands.
2. **"Can you prune this search?"** — A good answer names a condition under which a branch cannot possibly lead to a solution and abandons it early. Pruning is what separates backtracking from brute force, and interviewers ask for it directly.

---

## Session 2 (Wednesday) — Bit Manipulation

> Optional. Not graded. Skip freely.

### Check your understanding
1. What does `x & (x - 1)` do, and what is the classic use?
2. XOR: why does it find the single non-repeated element, and what property makes that work?
3. You need to iterate over every subset of a set of 20 elements. Write the loop.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 29 Divide Two Integers | Medium | 35 min |
| LC 137 Single Number II | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 201 Bitwise AND of Numbers Range | Medium | 40 min |
| LC 980 Unique Paths III | **Hard** | 55 min |

LC 201 is a bit problem where the answer is the common prefix of two numbers — obvious in binary, invisible in decimal. LC 980 is backtracking on a grid and belongs to Tuesday as much as today.

### 3. Revision — Week 14 (DP II) · Week 12 (Graphs II & Greedy) · Week 9 (Trees I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 389 Find the Difference | Easy | 25 min |
| LC 95 Unique Binary Search Trees II | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why does XOR work here?"** — A good answer gives the two properties — `x ^ x == 0` and XOR is commutative and associative — so pairs cancel regardless of order. Reciting the trick without those properties is the weak version.
2. **"What if the numbers could be negative?"** — A good answer raises two's complement and the language's shift behaviour — Python's arbitrary-precision integers versus JavaScript coercing to 32 bits. That JS trap is one of the eight in the cheatsheet.

---

## Session 3 (Thursday) — Data Structure Design

> Optional. Not graded. Skip freely.

### Check your understanding
1. LRU cache in O(1): what two structures, and what does each one provide?
2. Why does the linked list in an LRU cache need to be doubly linked?
3. You are asked to design a rate limiter. What are the first three questions you ask before designing anything?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 535 Encode and Decode TinyURL | Medium | 35 min |
| LC 449 Serialize and Deserialize BST | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 284 Peeking Iterator | Medium | 40 min |
| LC 352 Data Stream as Disjoint Intervals | **Hard** | 55 min |

### 3. Revision — Week 14 (DP II) · Week 12 (Graphs II & Greedy) · Week 9 (Trees I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1684 Count the Number of Consistent Strings | Easy | 25 min |
| LC 1980 Find Unique Binary String | Medium | 35 min |

### 4. Interview follow-ups
1. **"What are the operations and their complexities?"** — A good answer states the API first, then the complexity of each operation, then the structures that achieve it — in that order. Starting from the API rather than the data structure is what distinguishes a design answer from a coding answer.
2. **"How would this behave under concurrent access?"** — A good answer identifies which invariant breaks when two threads interleave, and proposes the coarsest lock that fixes it. Saying "I have not built concurrent systems, but the race is here" is a perfectly strong answer.

---

## Session 4 (Friday) — ARENA: Mock Interview Round 3

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Mock Round 3, full format. What are the five things you do in the first two minutes, in order?
2. Rounds 1 and 2 each gave you a written failure pattern. Name both. Are they the same one?
3. The interviewer asks something you do not know. Say out loud, now, what your first sentence is.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 89 Gray Code | Medium | 35 min |
| LC 784 Letter Case Permutation | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1930 Unique Length-3 Palindromic Subsequences | Medium | 40 min |
| LC 140 Word Break II | **Hard** | 55 min |

LC 140 is backtracking with memoisation — the two halves of this phase combined, and a fair final Hard.

### 3. Revision — Week 14 (DP II) · Week 12 (Graphs II & Greedy) · Week 9 (Trees I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2942 Find Words Containing Character | Easy | 25 min |
| LC 1110 Delete Nodes And Return Forest | Medium | 35 min |

### 4. Interview follow-ups
1. **"I'd like you to think out loud while you work."** — A good answer narrates the reasoning, including dead ends: "I'm considering a hash map here, but that loses the ordering, so...". Interviewers cannot score silence, and this is the most common reason competent candidates fail.
2. **"What would you change if you had another hour?"** — A good answer names something specific and real — a test case not covered, a naming choice, a complexity that could improve — rather than "nothing" or a vague apology. It shows judgement about your own work, which is the last thing the round measures.
