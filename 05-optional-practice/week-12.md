# Week 12 — Graphs II & Greedy · Optional Practice

**Phase II** · Week band: **Medium/Hard** · Reinforce = Easy/Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Three techniques that each look like a trick until you have done four problems with them. This is a week where the Reinforce track earns its place — Union-Find in particular is short enough to write from memory and only clicks through repetition.

---

## Session 1 (Tuesday) — Union-Find (Disjoint Set Union)

> Optional. Not graded. Skip freely.

### Check your understanding
1. Union-Find has two optimisations. Name both, and say what each one fixes.
2. Without path compression, what shape can the tree degenerate into, and what does `find` cost then?
3. You need the number of connected components. What do you track, and when do you update it?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 399 Evaluate Division | Medium | 35 min |
| LC 947 Most Stones Removed with Same Row or Column | Medium | 40 min |

Both are gentle **Mediums** — there is no such thing as an Easy Union-Find problem. LC 399 is Union-Find with a weight attached to each edge, which is the version people find genuinely surprising.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1254 Number of Closed Islands | Medium | 40 min |
| LC 827 Making A Large Island | **Hard** | 55 min |

### 3. Revision — Week 11 (Graphs I) · Week 9 (Trees I) · Week 6 (Stacks & Queues)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2236 Root Equals Sum of Children | Easy | 25 min |
| LC 1161 Maximum Level Sum of a Binary Tree | Medium | 35 min |

### 4. Interview follow-ups
1. **"What is the complexity of `find` with both optimisations?"** — A good answer says effectively constant — the inverse Ackermann function — and is comfortable saying "effectively O(1) for any realistic input" rather than reciting a name it cannot explain.
2. **"Could you solve this with DFS instead?"** — A good answer says yes for a static graph, and then gives the case where Union-Find wins: edges arriving one at a time, where DFS would mean re-traversing after every addition.

---

## Session 2 (Wednesday) — Dijkstra's Algorithm

> Optional. Not graded. Skip freely.

### Check your understanding
1. Dijkstra is BFS with what one change? Answer in a single sentence.
2. Why does Dijkstra fail on negative edge weights? Give a three-node example.
3. Why can the same node be pushed onto the heap several times, and what is the standard way to handle that?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 886 Possible Bipartition | Medium | 35 min |
| LC 2685 Count the Number of Complete Components | Medium | 40 min |

Gentle **Mediums** again.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2492 Minimum Score of a Path Between Two Cities | Medium | 40 min |
| LC 2092 Find All People With Secret | **Hard** | 55 min |

LC 2092 is a sorting problem plus Union-Find over time-ordered events — a good demonstration that Tuesday and Wednesday's tools compose.

### 3. Revision — Week 11 (Graphs I) · Week 9 (Trees I) · Week 6 (Stacks & Queues)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1022 Sum of Root To Leaf Binary Numbers | Easy | 25 min |
| LC 515 Find Largest Value in Each Tree Row | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why is Dijkstra O((V + E) log V)?"** — A good answer accounts for the heap: each edge can cause one push, each push costs log V, and each node is finalised once. Being able to attribute the log to the heap specifically is the strong answer.
2. **"What if all edge weights were equal?"** — A good answer says use plain BFS — the heap becomes pointless overhead. Recognising when the simpler tool suffices is a real signal.

---

## Session 3 (Thursday) — Greedy & the Exchange Argument

> Optional. Not graded. Skip freely.

### Check your understanding
1. Greedy needs an argument, not a hunch. What is an exchange argument, in two sentences?
2. Give a problem where the obvious greedy choice is wrong, and say what fixes it.
3. How do you decide what to sort by before a greedy scan? What is the general question to ask?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2037 Minimum Number of Moves to Seat Everyone | Easy | 20 min |
| LC 2864 Maximum Odd Binary Number | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 670 Maximum Swap | Medium | 40 min |
| LC 857 Minimum Cost to Hire K Workers | **Hard** | 55 min |

### 3. Revision — Week 11 (Graphs I) · Week 9 (Trees I) · Week 6 (Stacks & Queues)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 897 Increasing Order Search Tree | Easy | 25 min |
| LC 513 Find Bottom Left Tree Value | Medium | 35 min |

### 4. Interview follow-ups
1. **"Prove your greedy choice is safe."** — A good answer does the exchange: take any optimal solution that differs from your first greedy pick, swap yours in, and show the result is no worse. This is the single most-requested proof in interviews and it is learnable.
2. **"How do you know a greedy approach works here at all, rather than DP?"** — A good answer names the property — a locally optimal choice never has to be revisited — and admits when it cannot prove it. "I believe greedy works because X; if I'm wrong, the fallback is DP over the same states" is a strong, honest answer.

---

## Session 4 (Friday) — ARENA: Contest 7

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Union-Find, Dijkstra, or greedy? Give the trigger phrase for each.
2. From memory, in a blank file: Union-Find with both optimisations, then Dijkstra. This is the highest-value 30 minutes of the week.
3. Next week is DP, the hardest topic in the course. Is your recursion from Week 7 still solid? Answer honestly — DP is memoised recursion and nothing else.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2706 Buy Two Chocolates | Easy | 20 min |
| LC 2144 Minimum Cost of Buying Candies With Discount | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1007 Minimum Domino Rotations For Equal Row | Medium | 40 min |
| LC 2551 Put Marbles in Bags | **Hard** | 55 min |

### 3. Revision — Week 11 (Graphs I) · Week 9 (Trees I) · Week 6 (Stacks & Queues)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 783 Minimum Distance Between BST Nodes | Easy | 25 min |
| LC 752 Open the Lock | Medium | 35 min |

### 4. Interview follow-ups
1. **"This looked greedy but greedy fails. What now?"** — A good answer describes the state you would memoise instead, which is exactly the Week 13 conversation. Noticing that greedy has failed, and why, is more valuable than any specific fix.
2. **"Which of this week's three tools would you reach for first on an unfamiliar problem, and why?"** — A good answer keys off the question being asked — connectivity, shortest weighted path, or a single local optimum — rather than off the input shape. That is the pattern-recognition Checkpoint 2 was measuring.
