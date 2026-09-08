# Topic Map — 16 Weeks

---

## 1. Phase structure

| Phase | Weeks | Sessions | Intent |
|---|---|---|---|
| **I — Foundations** | 1–6 | 18 | The data structures a beginner must own, each taught *with* its interview patterns attached. Ends with a graded checkpoint. |
| **II — Techniques** | 7–13 | 21 | The algorithmic toolkit: recursion, binary search, trees, heaps, graphs, greedy, DP. This is where the course gets hard. |
| **III — Integration** | 14–16 | 9 | Hard synthesis, mixed pattern recognition, and full interview simulation. |

Difficulty band by phase: **I** = Easy → Easy/Medium · **II** = Medium → Medium/Hard · **III** = Medium/Hard → Hard.

---

## 2. The full sequence

| Wk | Theme | Tuesday | Wednesday | Thursday | Friday Arena |
|---|---|---|---|---|---|
| 1 | Toolkit & Complexity | Problem-solving frame; arrays & strings toolkit | **Hash maps & sets** | Time & space complexity | **Diagnostic contest** |
| 2 | Arrays I | Traversal patterns; Kadane's | Prefix sums & difference arrays | Hash-map counting patterns | Contest 1 |
| 3 | Two Pointers & Window | Opposite-end two pointers | Same-direction & fast/slow | **Sliding window template** | Contest 2 |
| 4 | Strings & Matrices | String ops, palindromes, anagrams | Sliding window on strings | 2D matrices & grid thinking | **Mock Round 1** |
| 5 | Linked Lists | **Classes/OOP inline**; Node basics | Reversal; Floyd's cycle detection | List surgery | Contest 3 |
| 6 | Stacks & Queues | Stack fundamentals | **Monotonic stack** | Deque & design problems | **Checkpoint 1** |
| 7 | **Recursion** | Call stack, base cases, tracing | Recursion on arrays/strings | Divide & conquer | Contest 4 |
| 8 | Binary Search & Sorting | The correct BS template | **Binary search on answer** | Sorting as a tool; intervals | Contest 5 |
| 9 | Trees I | Representation & DFS traversals | BFS, depth, diameter, paths | **BST properties** | **Mock Round 2** |
| 10 | Trees II · Heaps · Tries | Serialize; construct from traversals | **Heaps & top-K** | **Tries** | Contest 6 |
| 11 | Graphs I | Representation; grid-as-graph; DFS | BFS & multi-source BFS | **Topological sort** | **Checkpoint 2** |
| 12 | Graphs II & Greedy | **Union-Find** | **Dijkstra** | Greedy & exchange argument | Contest 7 |
| 13 | **DP I** | Memoization; state definition | 1D DP family | 2D / grid DP | Contest 8 |
| 14 | **DP II** | Knapsack family | String DP | State-machine & bitmask DP | Contest 9 |
| 15 | Backtracking · Bits · Design | **Backtracking template** | Bit manipulation | **Design round** | **Mock Round 3** |
| 16 | Simulation | Pattern-recognition drill | Full interview simulation | Weakness clinic & roadmap | **Final Assessment** |

---

## 3. Prerequisite dependency chain

The order is not arbitrary. These edges are load-bearing — breaking one breaks everything downstream.

```
Wk1 hash maps ──────┬──> Wk2 counting patterns
                    ├──> Wk4 anagram/window-on-strings
                    ├──> Wk10 tries (conceptually)
                    ├──> Wk11 graph adjacency lists + visited sets
                    └──> Wk13 memoization tables

Wk1 complexity ─────────> everything (students must cost their own code from Wk2 on)

Wk3 two pointers ───┬──> Wk5 fast/slow on linked lists
                    └──> Wk8 sorted-array techniques

Wk5 OOP / Node ─────┬──> Wk6 stack & queue implementation
                    ├──> Wk9 TreeNode
                    ├──> Wk10 TrieNode
                    └──> Wk15 LRU cache design

Wk6 stack ──────────┬──> Wk7 the call stack (concrete model for recursion)
                    └──> Wk9 iterative tree traversal

Wk7 RECURSION ──────┬──> Wk8 divide & conquer, merge sort
   ★ critical       ├──> Wk9 all tree work
                    ├──> Wk13 DP (memoized recursion IS the on-ramp)
                    └──> Wk15 backtracking

Wk9 tree DFS ───────┬──> Wk11 graph DFS (a tree is a graph without cycles)
                    └──> Wk14 DP on trees (flex)

Wk11 BFS ───────────────> Wk12 Dijkstra (BFS + a priority queue)

Wk10 heaps ─────────────> Wk12 Dijkstra
```

**The one edge to protect above all others: Week 7 → everything.** If recursion has not landed by the end of Week 7, Weeks 9, 13, 14 and 15 will fail. See `01-instructor/05-intervention-guide.md`.

---

## 4. Pattern coverage checklist

The ~20 patterns this course delivers. Each is defined with trigger signals and a template in `03-reference/02-pattern-catalogue.md`.

| # | Pattern | Introduced | Reinforced |
|---|---|---|---|
| 1 | Hash map lookup / complement | Wk 1–2 | throughout |
| 2 | Frequency counting & grouping | Wk 2 | Wk 4, 10 |
| 3 | Prefix sums & difference arrays | Wk 2 | Wk 13 |
| 4 | Kadane / running optimum | Wk 2 | Wk 13, 14 |
| 5 | Two pointers (opposite ends) | Wk 3 | Wk 4, 8 |
| 6 | Two pointers (same direction) | Wk 3 | Wk 5 |
| 7 | Fast & slow pointers | Wk 3 | Wk 5 |
| 8 | Sliding window (fixed & variable) | Wk 3 | Wk 4 |
| 9 | In-place array manipulation | Wk 3 | Wk 4 |
| 10 | Matrix / grid traversal | Wk 4 | Wk 11 |
| 11 | Linked-list pointer surgery | Wk 5 | Wk 15 |
| 12 | Monotonic stack / deque | Wk 6 | Wk 14 |
| 13 | Recursion & recursion trees | Wk 7 | Wk 9, 13, 15 |
| 14 | Divide & conquer | Wk 7 | Wk 8 |
| 15 | Binary search (array & answer space) | Wk 8 | Wk 12 |
| 16 | Sort-then-scan; interval merging | Wk 8 | Wk 12 |
| 17 | Tree DFS & BFS | Wk 9 | Wk 10, 11 |
| 18 | BST invariant exploitation | Wk 9 | Wk 10 |
| 19 | Heap / top-K | Wk 10 | Wk 12 |
| 20 | Trie / prefix tree | Wk 10 | Wk 15 |
| 21 | Graph traversal & connectivity | Wk 11 | Wk 12 |
| 22 | Topological sort | Wk 11 | — |
| 23 | Union-Find | Wk 12 | — |
| 24 | Dijkstra / weighted shortest path | Wk 12 | — |
| 25 | Greedy & exchange argument | Wk 12 | Wk 14 |
| 26 | Dynamic programming (6 families) | Wk 13–14 | Wk 16 |
| 27 | Backtracking | Wk 15 | Wk 16 |
| 28 | Bit manipulation | Wk 15 | — |
| 29 | Data-structure design | Wk 15 | Wk 16 |

---

## 5. The six DP families

DP is the largest single topic (Weeks 13–14, six sessions) because it is the most common reason strong candidates fail. Taught as **six named families**, not as "DP":

| Family | Week | Canonical problems |
|---|---|---|
| Linear / 1D | 13 | Climbing Stairs · House Robber · Decode Ways |
| Grid / 2D | 13 | Unique Paths · Minimum Path Sum |
| Subsequence | 13–14 | Longest Increasing Subsequence · LCS |
| Knapsack (0/1, unbounded) | 14 | Coin Change · Partition Equal Subset Sum |
| String / two-sequence | 14 | Edit Distance · Longest Palindromic Subsequence |
| State machine | 14 | Best Time to Buy and Sell Stock (all variants) |

Bitmask DP is introduced at awareness level in Week 14's flex block only.

---

## 6. In scope / out of scope

**In, at implementation depth:** arrays, strings, hash maps, sets, two pointers, sliding window, prefix sums, matrices, linked lists, stacks, queues, deques, monotonic structures, recursion, divide & conquer, binary search (both kinds), sorting as a tool, intervals, binary trees, BSTs, heaps, tries, graphs (DFS, BFS, topological sort, Union-Find, Dijkstra), greedy, the six DP families, backtracking, bit manipulation, data-structure design.

**In, at awareness depth only** — recognise and discuss, never implement: KMP · Rabin–Karp · balanced-BST rotation mechanics · bitmask DP.

**Out, deliberately:** segment trees · Fenwick/BIT · MST (Kruskal, Prim) · network flow · bipartite matching · suffix arrays and automata · formal Master-theorem drills · sorting-algorithm internals beyond merge and quick · system design · low-level design · behavioural rounds.

Rationale for each cut is in `01-curriculum-charter.md`, §4.
