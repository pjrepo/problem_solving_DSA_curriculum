# Week 11 — Graphs I: Traversal, BFS & Topological Sort

**Phase II: Techniques** · Difficulty band: **Medium** · Friday: **Checkpoint 2 (graded)**

> Graphs frighten students far more than they should. The truth to lead with: **they already know graph traversal.** Week 9's tree DFS is graph DFS; Week 9's level-order BFS is graph BFS. Only two things are genuinely new — a `visited` set (because graphs have cycles) and an adjacency list instead of `left`/`right`.
>
> Say that in the first five minutes. It converts the week from "a scary new topic" into "a small extension of something you can already do."

---

## Exit criteria

- [ ] Build an adjacency list from an edge list, and say when a matrix is preferable
- [ ] Treat a grid as a graph and write the direction-vector neighbour loop
- [ ] Write DFS and BFS on a graph with correct visited handling
- [ ] Explain why **BFS gives shortest path in unweighted graphs and DFS does not**
- [ ] Recognise and implement multi-source BFS
- [ ] Detect a cycle in a directed graph
- [ ] Write Kahn's topological sort, and explain why leftover nodes mean a cycle

---

## Session 1 (Tuesday) — Representation & DFS

### Weekend-set debrief (0:00–0:15)
Debrief **LC 212 Word Search II** — check the editorials for the inversion (build a trie, walk the grid once, prune). Then **LC 297**: ask what breaks without null markers.

### Concept spine (0:15–0:45)

**Part A — the reassurance, delivered first.**

Draw a binary tree, then draw a graph. *"What is actually different?"*

| | Tree | Graph |
|---|---|---|
| Children | exactly `left`, `right` | any number — an adjacency list |
| Cycles | impossible | possible → **you need a `visited` set** |
| Root | one | may be none; may be disconnected |
| Traversal | DFS, BFS | **the same DFS and BFS** |

> *"A tree is a connected graph with no cycles. You have been doing graph traversal for two weeks. Today you add a visited set and change how you find the neighbours."*

**Part B — representation.**

```python
from collections import defaultdict
graph = defaultdict(list)
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)        # omit this line for a DIRECTED graph
```

| | Adjacency list | Adjacency matrix |
|---|---|---|
| Space | O(V + E) | O(V²) |
| "Are u and v adjacent?" | O(degree) | **O(1)** |
| Iterate a node's neighbours | **O(degree)** | O(V) |
| Best for | sparse graphs (almost always) | dense graphs, or frequent edge queries |

*"Default to the adjacency list. Reach for a matrix only when the graph is dense or you query specific edges constantly."*

**Part C — a grid is a graph.** Planted in Week 4, cashed in now.

```python
DIRS = [(-1,0), (1,0), (0,-1), (0,1)]
for dr, dc in DIRS:
    nr, nc = r + dr, c + dc
    if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == '1':
        ...
```

*"The bounds check **is** the adjacency list. You do not build the graph — you compute the neighbours on demand."*

**Part D — the DFS template.**

```python
def dfs(node):
    if node in visited: return
    visited.add(node)
    for nxt in graph[node]:
        dfs(nxt)
```

**The `visited` set is the only new line versus a tree**, and it is what stops infinite recursion on a cycle. Ask what happens without it — then let someone run it and hit `RecursionError`.

**For grids**, marking the cell itself (`grid[r][c] = '0'`) is often used instead of a separate set. Mention the trade-off: it saves O(n) memory but mutates the caller's input, which is worth flagging aloud in an interview.

**Part E — connected components.** *"How many islands?"* → loop over every cell; each time you find an unvisited land cell, that is a new component — run DFS to consume all of it. **The loop over starting points is what counts components**, not the DFS itself. Students consistently miss this.

### Live-code (0:45–1:05)
**LC 200 Number of Islands** with DFS, then **LC 547 Number of Provinces** — the same algorithm on an adjacency matrix instead of a grid, to show the traversal is unchanged by representation.

### Guided practice (1:10–1:50)
1. **LC 733 Flood Fill** — Easy. DFS on a grid, minimal.
2. **LC 200 Number of Islands** — Medium
3. **LC 695 Max Area of Island** — Medium. DFS that returns a value.
4. **LC 547 Number of Provinces** — Medium

### Live critique (1:50–2:00)
An *LC 200*. Focus: where is the cell marked visited — before recursing, or after? Marking after the recursion causes repeated visits and, on a large grid, a stack overflow. Also ask whether they mutated the input grid and whether they would mention that in an interview.

### Flex (2:00–2:30)
**LC 133 Clone Graph** — Medium. DFS plus a hash map from original node to copy. It is Week 5's LC 138 (copy with random pointer) on a graph — point out the parallel.

### Common misconceptions
- No `visited` set → infinite recursion.
- Marking visited after the recursive call rather than before.
- Forgetting to loop over all starting nodes, so disconnected components are missed.
- Off-by-one in grid bounds checks.
- Using DFS for shortest path (Wednesday's topic).

### Assignment 11.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 733 Flood Fill | Easy | 20 min |
| LC 200 Number of Islands | Medium | 30 min |
| LC 695 Max Area of Island | Medium | 30 min |
| LC 547 Number of Provinces | Medium | 30 min |

---

## Session 2 (Wednesday) — BFS & Multi-Source BFS

### Warm-up (0:00–0:10)
1. What is the only structural difference between tree DFS and graph DFS?
2. Adjacency list or matrix for a sparse graph, and why?
3. What does the outer loop over all nodes accomplish in LC 200?

### Concept spine (0:10–0:45)

**Part A — BFS, and why it finds shortest paths.**

Put Week 9's level-order code on the board and change two lines:

```python
def bfs(start):
    q = deque([start])
    visited = {start}
    dist = 0
    while q:
        for _ in range(len(q)):          # one full level
            node = q.popleft()
            for nxt in graph[node]:      # <- was node.left / node.right
                if nxt not in visited:   # <- new: graphs have cycles
                    visited.add(nxt)
                    q.append(nxt)
        dist += 1
```

> *"That is the Week 9 code. Two lines changed."*

**Why BFS gives shortest paths in unweighted graphs:** it explores in order of distance — all nodes at distance 1, then all at distance 2, and so on. **The first time you reach a node, you have reached it by a shortest path.** DFS may reach it by a long wandering route first, so it offers no such guarantee.

**Crucial detail:** mark nodes visited **when enqueuing**, not when dequeuing. Otherwise a node can be enqueued many times before it is processed. Show the failure.

**Part B — multi-source BFS.** The idea students find least obvious and that interviewers ask most.

> **LC 994 Rotting Oranges.** Several oranges start rotten. How long until all are rotten?

*"How do you BFS from many starting points?"* Let them propose running BFS from each source and taking the minimum — correct but O(sources × V).

> **Seed the queue with all sources at once, at distance 0.** The BFS then expands from all of them simultaneously, and each cell is reached by whichever source is nearest.

```python
q = deque([(r, c) for r, c in all_sources])
```

*"One line. That is the entire technique, and it turns several problems from hard into routine."* **LC 542 (01 Matrix)** is the same idea: seed with every zero.

**Part C — implicit graphs.** The nodes need not exist as a data structure. In Word Ladder the nodes are words and the edges connect words differing by one letter — the graph is *computed*, never stored.

> *"Ask: what is a node? What is an edge? If you can answer those two questions, it is a graph problem, whatever it looks like."*

### Live-code (0:45–1:05)
**LC 994 Rotting Oranges** as multi-source BFS, tracing the wavefront on a grid. Then **LC 1091 Shortest Path in Binary Matrix** with 8-directional movement, to show the direction vector is a parameter, not a constant.

### Guided practice (1:10–1:50)
1. **LC 1091 Shortest Path in Binary Matrix** — Medium
2. **LC 994 Rotting Oranges** — Medium
3. **LC 542 01 Matrix** — Medium. Multi-source again; the recognition should now be quick.
4. **LC 130 Surrounded Regions** — Medium. The inversion: start from the *border* and mark what is safe, rather than trying to detect enclosure directly.

LC 130 is the thinking problem — the trick is to solve the complement.

### Live critique (1:50–2:00)
An *LC 994*. Focus: was visited marked at enqueue or dequeue? And did they handle the "some oranges are unreachable" case by checking for remaining fresh oranges at the end?

### Flex (2:00–2:30)
**LC 127 Word Ladder** — Hard. The implicit-graph archetype. Show how building neighbours via wildcard patterns (`h*t`) avoids comparing every pair of words.

### Common misconceptions
- Marking visited at dequeue time.
- Using a list with `pop(0)` as the queue.
- Running BFS once per source instead of seeding all sources.
- Believing DFS can find shortest paths.
- Forgetting that BFS's shortest-path guarantee requires **unweighted** edges (Week 12 handles weights).

### Assignment 11.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1091 Shortest Path in Binary Matrix | Medium | 30 min |
| LC 994 Rotting Oranges | Medium | 30 min |
| LC 542 01 Matrix | Medium | 30 min |
| LC 417 Pacific Atlantic Water Flow | Medium | 35 min |

LC 417 uses the same "start from the border" inversion as LC 130 — deliberately paired.

---

## Session 3 (Thursday) — Cycle Detection & Topological Sort

### Warm-up (0:00–0:10)
1. Why does BFS find shortest paths but DFS does not?
2. How do you BFS from many sources at once?
3. In LC 130, why start from the border?

### Concept spine (0:10–0:45)

**Part A — the motivating problem.** *"You have courses with prerequisites. Can you finish them all, and in what order?"*

This is a **directed** graph. Two questions: is there a cycle (an impossible circular dependency), and if not, what is a valid order?

**Part B — Kahn's algorithm.**

> *"A course you can take right now is one with no unmet prerequisites — **in-degree zero**. Take it, remove it, and see which courses that unblocks. Repeat."*

```python
def topo_sort(n, edges):
    graph = defaultdict(list)
    indeg = [0] * n
    for u, v in edges:            # u must come before v
        graph[u].append(v)
        indeg[v] += 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    order = []
    while q:
        node = q.popleft()
        order.append(node)
        for nxt in graph[node]:
            indeg[nxt] -= 1
            if indeg[nxt] == 0:
                q.append(nxt)

    return order if len(order) == n else []     # short => a cycle exists
```

**The last line is the whole cycle-detection story.** If nodes remain unprocessed, every one of them is still waiting on something — which can only happen inside a cycle. Have a student explain that back before moving on.

Complexity: **O(V + E)**.

**Part C — cycle detection with DFS (three colours).** Worth showing as the alternative:

- **White** — unvisited
- **Grey** — on the current recursion stack
- **Black** — fully explored

**Reaching a grey node means a cycle**, because you have looped back to something still open on your own path. *"Grey means 'I am currently inside this node's exploration.' Meeting it again means you went in a circle."*

**Distinguish this from undirected cycle detection**, where simply meeting a visited node that is not your parent is enough. The distinction is a classic interview follow-up.

**Part D — recognition signals.** Put on the board:

| Statement says | Reach for |
|---|---|
| "prerequisites", "dependencies", "build order" | topological sort |
| "can all tasks be completed?" | cycle detection |
| "is this schedule possible?" | cycle detection |
| "find a valid ordering" | topological sort |

### Live-code (0:45–1:05)
**LC 207 Course Schedule** with Kahn's algorithm, then **LC 210** — which is the same code returning `order` instead of a boolean. The near-zero cost of the second problem is the point.

### Guided practice (1:10–1:50)
1. **LC 207 Course Schedule** — Medium
2. **LC 210 Course Schedule II** — Medium
3. **LC 802 Find Eventual Safe States** — Medium. Cycle detection with a twist: reverse the edges and it becomes a topological sort.

### Live critique (1:50–2:00)
An *LC 207*. Focus: did they check `len(order) == n`, or try to detect the cycle some other way? And did they build the edge direction correctly — reversing it is the most common bug and passes some tests by luck.

### Flex (2:00–2:30) — Checkpoint 2 preparation
Not new content. Six unlabelled problems from Weeks 1–11 on the projector; students name the pattern and the trigger only, no coding. Same drill as before Checkpoint 1, now across a much wider space.

### Common misconceptions
- Building edges backwards (`v → u` instead of `u → v`).
- Forgetting the completeness check, so cycles go undetected.
- Applying undirected cycle detection to a directed graph.
- Believing the topological order is unique — it usually is not.

### Assignment 11.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 207 Course Schedule | Medium | 30 min |
| LC 210 Course Schedule II | Medium | 30 min |
| LC 802 Find Eventual Safe States | Medium | 40 min |
| **Written:** explain in 5 sentences why leftover nodes in Kahn's algorithm prove a cycle exists | — | 20 min |

---

## Session 4 (Friday) — ARENA: CHECKPOINT 2 (graded)

**Mode:** Checkpoint · covers **Weeks 1–11** · **8% of the final grade**

### Format
- **3 problems, 2 hours, individual, unlabelled**
- Each submission must include working code, stated complexity **with justification**, and one sentence naming the pattern and its trigger
- Graded on the code review rubric, 25 points
- **Passing bar: solve 2 of 3**

### Problems
| # | Problem | Difficulty | Pattern (not shown to students) |
|---|---|---|---|
| 1 | LC 442 Find All Duplicates in an Array | Medium | In-place index marking (Weeks 1–3) |
| 2 | LC 841 Keys and Rooms | Medium | Graph traversal (Week 11) |
| 3 | LC 826 Most Profit Assigning Work | Medium | Sort + two pointers / binary search (Weeks 3, 8) |

All three are fresh. Each targets a different phase of the course, so the result tells you *where* a student's gap is.

### After the checkpoint
Compare each student against their Checkpoint 1 score. The important signal is **direction**, not level:

- **Improved** — the system is working for them
- **Flat** — they are keeping up but not consolidating; check their red list and editorial quality
- **Declined** — almost always volume without understanding. Cut their load and go deeper (Mode D in `01-instructor/05-intervention-guide.md`)

Weeks 13–15 are the hardest of the course. **Any student who is struggling now needs the conversation this week**, not after Week 13.

### Assignment 11.4 (2h)
| Task | Budget |
|---|---|
| Re-solve any checkpoint problem you did not complete | 45 min |
| **Written:** for each problem, name the pattern and its trigger; if you missed it, what you saw instead | 30 min |
| Solve **LC 863 All Nodes Distance K in Binary Tree** — Medium | 45 min |

LC 863 is the perfect post-checkpoint problem: convert a tree into an undirected graph by adding parent links, then BFS. It fuses Weeks 9 and 11 in one move.

---

## Weekend Set 11 (6h) — due Tuesday, Week 12

Slightly lighter — Checkpoint 2 was Friday and Week 12 is dense.

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 133 Clone Graph | Medium | 35 min |
| LC 130 Surrounded Regions | Medium | 35 min |
| LC 310 Minimum Height Trees | Medium | 45 min |
| LC 1466 Reorder Routes to Make All Paths Lead to the City Zero | Medium | 35 min |
| LC 127 Word Ladder | Hard | 55 min |

**LC 310** is a topological-sort-flavoured peeling of leaves layer by layer — an unusual and instructive application. **LC 1466** requires treating a directed graph as undirected for traversal while remembering each edge's true direction, which is a genuinely useful trick.

### Spaced revision (1h) — Weeks 10, 8, 5
| Problem | Source | Target time |
|---|---|---|
| LC 208 Implement Trie | Week 10 (N−1) | under 20 min |
| LC 875 Koko Eating Bananas | Week 8 (N−3) | under 15 min |

### Written editorial (1h)
**LC 127 Word Ladder.**

The observation to look for: *"The graph is never built — words are nodes and an edge exists between words differing by one letter, so BFS from the start word gives the shortest transformation. To avoid comparing every pair of words (O(n²)), index words by wildcard patterns like `h*t`, so neighbours are found by dictionary lookup."*

Both halves matter: recognising the **implicit graph**, and the pattern-indexing that keeps neighbour lookup cheap. A student who gets only the first has the right idea with the wrong complexity — which is worth saying to them explicitly.

---

## Instructor notes

### What usually goes wrong this week
- **Students think graphs are a huge new topic.** The Tuesday reassurance is the single most useful thing you say this week. Repeat it Wednesday.
- **Marking visited at dequeue time** in BFS. Catch it in critique — it is subtle and passes small tests.
- **Multi-source BFS does not land on first exposure.** LC 994 and LC 542 back to back is the fix.
- **Kahn's edge direction gets reversed.** Have students state, in words, what an edge means *before* they write the loop.
- **Checkpoint 2 lands in a heavy week.** That is why Week 11's assignments are the lightest of Phase II — do not add to them.

### Watch list
Compare Checkpoint 2 with Checkpoint 1 per student and act on the direction. This is your last clean measurement before the DP block, and DP is where a shaky foundation becomes visible to everyone.

### What to cut if you are behind
1. Tuesday's flex (LC 133) — it is in the weekend set
2. LC 802 from Thursday — LC 207 and LC 210 carry topological sort
3. LC 310 and LC 1466 from the weekend set
4. The three-colour DFS cycle detection — Kahn's alone is sufficient for interviews

**Never cut:** the "you already know graph traversal" framing, the multi-source BFS single line, the `len(order) == n` cycle argument, or Thursday's flex recognition drill.
