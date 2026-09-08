# Week 12 — Graphs II: Union-Find, Dijkstra & Greedy

**Phase II: Techniques** · Difficulty band: **Medium/Hard**

> Two specialised graph tools plus the last major technique before dynamic programming. **Greedy is placed immediately before DP deliberately** — the question "when is a locally optimal choice safe?" is exactly the question DP answers when the answer is "it isn't." Thursday's session ends with a greedy algorithm that *fails*, and Week 13 opens by fixing it.

---

## Exit criteria

- [ ] Implement Union-Find with path compression and union by size
- [ ] Say when Union-Find beats DFS for connectivity, and when it does not
- [ ] Explain why Dijkstra is "BFS with a priority queue" and why plain BFS fails on weighted graphs
- [ ] Implement Dijkstra with `heapq`, including the stale-entry check
- [ ] State an exchange argument justifying a greedy choice
- [ ] Produce a counterexample where greedy fails, and say what to use instead

---

## Session 1 (Tuesday) — Union-Find (Disjoint Set Union)

### Weekend-set debrief (0:00–0:15)
Debrief **LC 127 Word Ladder** — check for both halves: the implicit graph, and the wildcard indexing that keeps neighbour lookup cheap. Then return Checkpoint 2 results with individual written feedback. Have the "direction" conversation with anyone who declined.

### Concept spine (0:15–0:45)

**Part A — the motivating problem.** *"Friendships arrive one at a time. After each one, tell me whether A and B are now connected."*

*"Could you use DFS?"* → yes, but re-running it after every edge is O(V+E) per query.

> **Union-Find keeps a live answer as edges arrive.** Each group has a representative; two nodes are connected exactly when they share one.

**Part B — build it up, naively first.**

```python
parent = list(range(n))              # everyone is their own representative

def find(x):
    while parent[x] != x:
        x = parent[x]
    return x

def union(a, b):
    parent[find(a)] = find(b)
```

*"What is the worst case?"* → a chain, so `find` is O(n). Draw the degenerate tree.

**Two optimisations, each with a one-line idea:**

**Path compression** — on the way back up, point every node directly at the root.
```python
def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])      # flatten as we return
    return parent[x]
```

**Union by size** — always attach the smaller tree under the larger, so trees stay shallow.

```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.count = n                   # number of components

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb: return False        # already connected — often the answer itself
        if self.size[ra] < self.size[rb]: ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        self.count -= 1
        return True
```

**With both optimisations, operations are effectively O(1) amortised.** (The precise bound involves the inverse Ackermann function — mention the name once, note it is under 5 for any input in this universe, and move on. **Do not derive it.**)

**`union` returning `False` when already connected is not a detail — it is frequently the entire answer**, as in LC 684 (redundant connection): the first edge whose union fails is the one closing a cycle.

**Part C — when to use which.** This is a designed discrimination (see `01-instructor/04-spaced-revision-system.md`, §5):

| Situation | Use |
|---|---|
| Edges arrive incrementally; connectivity queried repeatedly | **Union-Find** |
| Static graph; you need the traversal itself (paths, distances, order) | **DFS / BFS** |
| Just counting components on a fixed graph | either — DFS is usually simpler to write |
| "Detect the edge that creates a cycle" | **Union-Find** |

**Part D — re-solve LC 547.** They solved Number of Provinces with DFS last week. Solve it again with DSU, in four lines. Then ask which they would present in an interview and why — the answer is genuinely "either, and here is the trade-off."

### Live-code (0:45–1:05)
The full `DSU` class, then **LC 547 with DSU**, then **LC 684 Redundant Connection** — where the answer is simply the first edge whose `union` returns `False`.

### Guided practice (1:10–1:50)
1. **LC 547 Number of Provinces** — Medium. Re-solved with DSU.
2. **LC 684 Redundant Connection** — Medium
3. **LC 1319 Number of Operations to Make Network Connected** — Medium. Needs `count` and a cable-surplus check.
4. **LC 990 Satisfiability of Equality Equations** — Medium. Union all the `==` constraints first, then verify every `!=`. Order matters, and that is the insight.

### Live critique (1:50–2:00)
An *LC 990*. Focus: did they process all equalities before any inequalities? Doing it in input order fails, and the failure is instructive — some algorithms require a processing order that the problem statement does not hand you.

### Flex (2:00–2:30)
**LC 721 Accounts Merge** — Medium. DSU over email addresses. The mapping from strings to DSU indices is the fiddly part, and it is realistic.

### Common misconceptions
- `find` without path compression, then claiming near-O(1).
- Comparing `parent[a] == parent[b]` instead of `find(a) == find(b)`.
- Forgetting to decrement the component count on a successful union.
- Using DSU where a traversal is actually needed (it gives you components, not paths).

### Assignment 12.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 547 Number of Provinces (DSU version) | Medium | 25 min |
| LC 684 Redundant Connection | Medium | 30 min |
| LC 1319 Number of Operations to Make Network Connected | Medium | 30 min |
| LC 990 Satisfiability of Equality Equations | Medium | 35 min |

---

## Session 2 (Wednesday) — Dijkstra's Algorithm

### Warm-up (0:00–0:10)
1. What do path compression and union by size each fix?
2. What does it mean when `union(a, b)` returns `False`?
3. Union-Find or DFS: "detect the edge that creates a cycle"?

### Concept spine (0:10–0:45)

**Part A — why BFS breaks.** Draw a small weighted graph where the shortest path in *edges* is not the shortest in *weight*:

```
      A --1-- B --1-- C
      |               |
      +------5--------+
```
BFS from A reaches C in one edge (cost 5). The real shortest path is A→B→C (cost 2).

> *"BFS explores by number of edges. When edges have different costs, that ordering is simply the wrong ordering."*

**Part B — the fix, stated in one sentence.**

> **Dijkstra is BFS with a priority queue instead of a plain queue: always expand the *nearest unvisited* node rather than the *earliest enqueued* one.**

That single substitution is the entire algorithm. Write BFS on the board and change `deque` to `heapq`.

```python
import heapq

def dijkstra(graph, start, n):
    dist = {start: 0}
    pq = [(0, start)]                       # (distance_so_far, node)
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist.get(node, float('inf')):
            continue                        # STALE entry — skip it
        for nxt, w in graph[node]:
            nd = d + w
            if nd < dist.get(nxt, float('inf')):
                dist[nxt] = nd
                heapq.heappush(pq, (nd, nxt))
    return dist
```

**Two lines need justification:**

- **The stale check.** A node can be pushed several times as better distances are found. Rather than deleting old entries (heaps do not support that), we push duplicates and skip any entry worse than the best known. **This is "lazy deletion" — the same trick mentioned in Week 10's sliding-window median.**
- **Popping means finalised.** The first time a node is popped, its distance is final. That is the correctness guarantee, and it is why Dijkstra needs **non-negative weights** — a negative edge could improve a node after it was finalised.

**Complexity:** O((V + E) log V).

**Part C — when *not* to use it.**

| Situation | Use |
|---|---|
| Unweighted, or all weights equal | **plain BFS** — simpler and faster |
| Weights are only 0 and 1 | 0-1 BFS with a deque (mention only) |
| Negative weights | Bellman–Ford (**name it, do not teach it**) |
| Non-negative weights | **Dijkstra** |

*"Reaching for a heap when all the weights are 1 is a small but real interview mistake — it says you pattern-matched on 'shortest path' instead of reading the problem."*

### Live-code (0:45–1:05)
**LC 743 Network Delay Time** in full, including the stale check. Then **LC 1631 Path With Minimum Effort**, where the "distance" being minimised is the *maximum single step* rather than a sum — showing the relaxation rule is a parameter you choose.

### Guided practice (1:10–1:50)
1. **LC 743 Network Delay Time** — Medium
2. **LC 1514 Path with Maximum Probability** — Medium. *Maximising* a product; negate the log, or just use a max-heap. Good for showing the framework generalises.
3. **LC 1631 Path With Minimum Effort** — Medium

### Live critique (1:50–2:00)
A *LC 743*. Focus: is the stale check present? Without it the code is still *correct* but can degrade badly on dense graphs. A good example of a subtle efficiency bug that tests do not catch.

### Flex (2:00–2:30)
**LC 787 Cheapest Flights Within K Stops** — Medium. Dijkstra's usual state (just the node) is insufficient — you need `(node, stops_used)`. **Enlarging the state to make an algorithm apply is a genuinely important idea, and it is exactly what DP asks for next week.** Worth the full block.

### Common misconceptions
- Omitting the stale check.
- Using Dijkstra on unweighted graphs.
- Applying it with negative weights.
- Marking visited at push time rather than relying on the distance check.
- Forgetting that "shortest" may mean something other than a sum.

### Assignment 12.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 743 Network Delay Time | Medium | 35 min |
| LC 1514 Path with Maximum Probability | Medium | 30 min |
| LC 1631 Path With Minimum Effort | Medium | 35 min |
| LC 787 Cheapest Flights Within K Stops | Medium | 40 min |

---

## Session 3 (Thursday) — Greedy & the Exchange Argument

> The bridge into dynamic programming. End this session with a greedy algorithm that **fails**.

### Warm-up (0:00–0:10)
1. What single change turns BFS into Dijkstra?
2. Why does Dijkstra require non-negative weights?
3. What is the state in LC 787, and why isn't the node alone enough?

### Concept spine (0:10–0:45)

**Part A — what greedy is.**

> Make the locally best choice at each step and never reconsider it.

*"The algorithm is always trivial. The difficulty is entirely in knowing whether it is correct."*

**Part B — the exchange argument.** The standard way to justify a greedy choice, and the thing interviewers actually probe:

> *Take any optimal solution. Show you can transform it, step by step, into the greedy solution without ever making it worse. Therefore the greedy solution is also optimal.*

Apply it to Week 8's interval scheduling: *"Suppose an optimal schedule picks some interval X first. The greedy picks the one finishing earliest, E. Since E finishes no later than X, swapping X for E leaves at least as much room for everything after — so the swapped schedule is still optimal. Repeat, and you have converted the optimum into the greedy solution."*

**Have a student reproduce that argument for a different problem.** It is the skill, not the anecdote.

**Part C — three greedy classics, derived.**

**LC 55 Jump Game.** Track the furthest index reachable so far; if you ever stand beyond it, you are stuck.
```python
reach = 0
for i, jump in enumerate(nums):
    if i > reach: return False
    reach = max(reach, i + jump)
return True
```

**LC 45 Jump Game II.** Minimum jumps. *"Think of it as BFS by levels: from everything reachable in k jumps, what is reachable in k+1?"* — the same level idea as Week 9's `level_size`.

**LC 134 Gas Station.** Two observations, both worth deriving:
1. If total gas ≥ total cost, a solution **must** exist
2. If you run out between `start` and `i`, then **no station between them can be a valid start** — each had at least as much surplus on arrival. So jump `start` to `i+1`.

That second observation is a genuine proof and makes the algorithm O(n) rather than O(n²).

**Part D — where greedy fails. Do this last, and deliberately.**

> **Coin change.** Coins `[1, 3, 4]`, make 6.
>
> Greedy takes the largest first: 4, then 1, then 1 → **three coins**.
> Optimal: 3 + 3 → **two coins**.

Let the room verify it. Then:

> *"Greedy fails here because taking the 4 forecloses a better combination. There is no exchange argument — and there cannot be, because the claim is false. When a locally optimal choice can be **regretted**, you need something that keeps all options open. That is dynamic programming, and it starts on Tuesday."*

**Ending Phase II's last technique on a failure is the best possible setup for Week 13.**

### Live-code (0:45–1:05)
**LC 55 Jump Game**, then **LC 45 Jump Game II** with the level framing.

### Guided practice (1:10–1:50)
1. **LC 55 Jump Game** — Medium
2. **LC 45 Jump Game II** — Medium
3. **LC 134 Gas Station** — Medium
4. **LC 621 Task Scheduler** — Medium. The formula-based solution needs a real argument about idle slots; the heap simulation is easier to justify. Show both.

### Live critique (1:50–2:00)
An *LC 134*. Focus: **can the author justify the jump to `i+1`?** Most will have coded it from a remembered solution. This is the week's central skill, so push on it.

### Flex (2:00–2:30)
**Greedy versus DP discrimination drill.** Six problems on the projector; for each, students decide *greedy or DP* and justify it in one sentence. No coding. This is direct preparation for Week 13 and the best use of the block.

### Common misconceptions
- Assuming greedy works because it passes the sample tests.
- Confusing "sort first, then be greedy" with "greedy is just sorting."
- No justification for the greedy choice.
- Believing DP is always needed — greedy is faster when it is valid.

### Assignment 12.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 55 Jump Game | Medium | 25 min |
| LC 45 Jump Game II | Medium | 30 min |
| LC 134 Gas Station | Medium | 35 min |
| LC 621 Task Scheduler | Medium | 35 min |

---

## Session 4 (Friday) — ARENA: Contest 7

**Mode:** Contest · 90 minutes

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 1971 Find if Path Exists in Graph | Easy | DSU or BFS — either is fine |
| P2 | LC 1020 Number of Enclaves | Medium | Border-inversion DFS (Week 11) |
| P3 | LC 881 Boats to Save People | Medium | Greedy + two pointers, never taught |
| P4 | LC 778 Swim in Rising Water | Hard | Dijkstra, or binary search + DSU |

**Calibration:** P3 is the transfer test — a greedy nobody has seen, where the exchange argument is easy to state once found. P4 has two legitimate solutions using different tools from this week, which makes it an excellent reveal.

### Reveal (1:45–2:00)
For **P4**, show *both* solutions and compare: Dijkstra where the "distance" is the maximum elevation on the path, versus binary searching the water level with DSU connectivity as the feasibility check. *"Two techniques from the same week, both correct. Being able to say that in an interview is worth more than either solution."*

### Assignment 12.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 778 Swim in Rising Water** — present **both** approaches | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 12 (6h) — due Tuesday, Week 13

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 721 Accounts Merge | Medium | 45 min |
| LC 785 Is Graph Bipartite? | Medium | 40 min |
| LC 406 Queue Reconstruction by Height | Medium | 40 min |
| LC 678 Valid Parenthesis String | Medium | 40 min |
| LC 502 IPO | Hard | 50 min |

**LC 785** is graph colouring via BFS/DFS — a genuinely common interview question. **LC 406** is a greedy whose *sort order* is the entire insight. **LC 678** has an elegant greedy (track a range of possible open-paren counts) that beats the DP solution, and comparing the two is a fitting close to Phase II.

### Spaced revision (1h) — Weeks 11, 9, 6
| Problem | Source | Target time |
|---|---|---|
| LC 994 Rotting Oranges | Week 11 (N−1) | under 20 min |
| LC 84 Largest Rectangle in Histogram | Week 6 (N−6) | under 30 min |

LC 84 is deliberately chosen: it was the hardest problem of Phase I, and re-solving it now should feel noticeably easier. Ask students to note the time difference in their tracker — visible evidence that the system works.

### Written editorial (1h)
**LC 134 Gas Station.**

The observation to look for: *"If total gas ≥ total cost a solution must exist. And if you run out between station `start` and station `i`, no station in between can be a valid start — each of them arrived with at least as much fuel as you had, so they would run out no later. Therefore the next candidate start is `i+1`, and one pass suffices."*

**This is the term's clearest test of whether a student can construct a correctness argument rather than describe code.** Read all 14 carefully.

---

## Instructor notes

### What usually goes wrong this week
- **DSU is memorised, not understood.** The tell: they cannot explain what `union` returning `False` means. Make it a critique question.
- **The stale check in Dijkstra gets omitted** and nobody notices, because it still produces correct answers. Point it out explicitly.
- **Greedy justification is skipped entirely.** Students code the remembered solution. The LC 134 critique and editorial exist to force it — do not let them off.
- **The coin change counterexample must be the last thing on Thursday.** It is the hinge into Week 13; delivering it early wastes it.

### Watch list
Thursday's greedy-versus-DP drill is a good predictor of Week 13. A student who cannot articulate why greedy fails on `[1,3,4]` making 6 will define DP states badly. Flag those names now and watch them in Week 13's Tuesday session.

### What to cut if you are behind
1. Wednesday's flex (LC 787) — but it previews state enlargement, so prefer to cut elsewhere
2. LC 990 from Tuesday
3. LC 502 and LC 406 from the weekend set
4. LC 621 from Thursday — LC 134 carries the justification lesson

**Never cut:** the exchange argument, the coin change counterexample, the "BFS with a priority queue" framing, or Thursday's greedy-versus-DP drill.
