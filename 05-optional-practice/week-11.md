# Week 11 — Graphs I · Optional Practice

**Phase II** · Week band: **Medium** · Reinforce = Easy/Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> **A note on this week's Reinforce track.** Genuinely *Easy* graph problems barely exist — a graph problem is Medium almost by definition, and the handful of Easy ones are in the standard set. So Monday and Tuesday's Reinforce picks are **tree** problems, and that is deliberate rather than a compromise: a tree is a graph without cycles, so the traversal is identical and only the visited-set bookkeeping is missing. If graphs feel hard, practising the traversal on the simpler structure is exactly the right move.
>
> **Friday is Checkpoint 2 — graded, and it covers Weeks 1–11.** Week 11 is already the heaviest week in the course. Skipping this file entirely this week is the recommended choice.

---

## Session 1 (Tuesday) — Representation & DFS

> Optional. Not graded. Skip freely.

### Check your understanding
1. Adjacency list versus adjacency matrix: give the space cost of each, and the input shape that makes the matrix the right choice.
2. A grid is a graph. What are the nodes, what are the edges, and what replaces the adjacency list?
3. Why does graph DFS need a visited set when tree DFS does not?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 997 Find the Town Judge | Easy | 20 min |
| LC 1791 Find Center of Star Graph | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1905 Count Sub Islands | Medium | 40 min |
| LC 2360 Longest Cycle in a Graph | **Hard** | 55 min |

LC 1905 is grid-as-graph exactly as taught today; LC 2360 is cycle detection on a functional graph and previews Thursday.

### 3. Revision — Week 10 (Trees II · Heaps · Tries) · Week 8 (Binary Search & Sorting) · Week 5 (Linked Lists)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1710 Maximum Units on a Truck | Easy | 25 min |
| LC 430 Flatten a Multilevel Doubly Linked List | Medium | 35 min |

### 4. Interview follow-ups
1. **"Where do you mark a node visited — before or after the recursive call?"** — A good answer says before, on entry, and gives the failure mode otherwise: the same node gets queued or entered many times, and on a cyclic graph you never terminate.
2. **"What is the complexity in terms of the graph?"** — A good answer gives O(V + E) and says what V and E are for *this* problem — for an m×n grid, V is mn and E is about 4mn. Quoting O(n²) for a grid without saying what n means is the vague answer interviewers probe.

---

## Session 2 (Wednesday) — BFS & Multi-Source BFS

> Optional. Not graded. Skip freely.

### Check your understanding
1. BFS finds shortest paths on an unweighted graph. Why does that break the moment edges have different weights?
2. Multi-source BFS: what is different about the initial state, and why does that still give correct shortest distances?
3. When does BFS need to track a level explicitly, and when can you get away with a distance map?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 559 Maximum Depth of N-ary Tree | Easy | 20 min |
| LC 993 Cousins in Binary Tree | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 934 Shortest Bridge | Medium | 40 min |
| LC 1293 Shortest Path in a Grid with Obstacles Elimination | **Hard** | 55 min |

LC 934 is DFS to find one island, then multi-source BFS to grow towards the other — both of this week's ideas in one problem, and the best single exercise here.

### 3. Revision — Week 10 (Trees II · Heaps · Tries) · Week 8 (Binary Search & Sorting) · Week 5 (Linked Lists)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1491 Average Salary Excluding the Minimum and Maximum Salary | Easy | 25 min |
| LC 164 Maximum Gap | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why is BFS correct for shortest paths?"** — A good answer argues by level: every node at distance d is dequeued before any node at distance d+1, so the first time you reach a node is by a shortest path. That argument is what Dijkstra generalises next week.
2. **"Why multiple sources at once rather than a BFS from each?"** — A good answer contrasts O(V + E) with O(k(V + E)) and explains that seeding the queue with all sources computes the distance to the *nearest* source in one pass.

---

## Session 3 (Thursday) — Cycle Detection & Topological Sort

> Optional. Not graded. Skip freely.

### Check your understanding
1. Topological sort exists only for which kind of graph? What does it mean if the algorithm cannot finish?
2. Kahn's algorithm uses in-degrees. What is the invariant that makes it correct?
3. Detecting a cycle in a *directed* graph needs three node states, not two. Name them, and say why two is not enough.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1557 Minimum Number of Vertices to Reach All Nodes | Medium | 35 min |
| LC 2115 Find All Possible Recipes from Given Supplies | Medium | 40 min |

Gentle **Medium** pair — see the note at the top of this file about Easy graph problems.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1462 Course Schedule IV | Medium | 40 min |
| LC 1298 Maximum Candies You Can Get from Boxes | **Hard** | 55 min |

### 3. Revision — Week 10 (Trees II · Heaps · Tries) · Week 8 (Binary Search & Sorting) · Week 5 (Linked Lists)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2418 Sort the People | Easy | 25 min |
| LC 539 Minimum Time Difference | Medium | 35 min |

### 4. Interview follow-ups
1. **"How do you know there is a cycle?"** — A good answer, for Kahn's: fewer nodes emitted than exist. For DFS: an edge to a node currently on the recursion stack — and it distinguishes that from an edge to a node merely already visited, which is fine.
2. **"Is the topological order unique?"** — A good answer says usually not, gives a two-node example with no edge between them, and notes that when a unique order is required the graph must be a chain — a real follow-up worth being ready for.

---

## Session 4 (Friday) — ARENA: CHECKPOINT 2 (graded)

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Checkpoint 2 day, covering Weeks 1–11 with **unlabelled** problems. For each signal, name the technique: "shortest number of steps", "is it possible to order these", "how many separate regions", "k largest".
2. Of Weeks 1–11, which two are weakest for you? That is what to revise tonight — not this file.
3. From memory: BFS on a grid, and DFS with a visited set. Both, in a blank file, before the checkpoint.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2101 Detonate the Maximum Bombs | Medium | 35 min |
| LC 1615 Maximal Network Rank | Medium | 40 min |

Gentle **Medium** pair. Honestly: do not use these today. Revise Weeks 1–10 instead.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2359 Find Closest Node to Given Two Nodes | Medium | 40 min |
| LC 2392 Build a Matrix With Conditions | **Hard** | 55 min |

### 3. Revision — Week 10 (Trees II · Heaps · Tries) · Week 8 (Binary Search & Sorting) · Week 5 (Linked Lists)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2529 Maximum Count of Positive Integer and Negative Integer | Easy | 25 min |
| LC 1833 Maximum Ice Cream Bars | Medium | 35 min |

### 4. Interview follow-ups
1. **"You were not told this was a graph problem. What gave it away?"** — A good answer names the reframing — cells as nodes, prerequisites as edges, states as nodes — because recognising that something *is* a graph is most of the difficulty by this point in the course.
2. **"Your DFS is recursive and the graph has 10⁵ nodes. Any concerns?"** — A good answer raises recursion depth immediately and offers the iterative version with an explicit stack. On grid problems with long snake-like regions this is a genuine failure, not a hypothetical.
