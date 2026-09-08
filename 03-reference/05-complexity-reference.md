# Complexity Reference

Built with the class in Week 1, Session 3. Students should be able to reproduce most of the first table from memory by Week 4.

---

## 1. Growth rates, with real numbers

The table that makes Big-O land. Approximate operation counts:

| n | O(log n) | O(n) | O(n log n) | O(n²) | O(2ⁿ) | O(n!) |
|---|---|---|---|---|---|---|
| 10 | 3 | 10 | 33 | 100 | 1,024 | 3.6M |
| 100 | 7 | 100 | 664 | 10,000 | 10³⁰ | — |
| 1,000 | 10 | 1,000 | 9,966 | 10⁶ | — | — |
| 100,000 | 17 | 10⁵ | 1.7M | 10¹⁰ | — | — |
| 1,000,000 | 20 | 10⁶ | 2×10⁷ | 10¹² | — | — |

**At a million elements, O(n) finishes instantly and O(n²) runs for days.** That gap is the entire reason this course exists.

---

## 2. The interview shortcut

Roughly 10⁸ simple operations per second is a fair working estimate. So the constraints tell you the intended solution **before you have had an idea**:

| Constraint | Target complexity | Likely technique |
|---|---|---|
| n ≤ 12 | O(n!) | permutations, brute force |
| n ≤ 20 | O(2ⁿ) | subsets, bitmask DP |
| n ≤ 100 | O(n³) | 3D DP, triple loops |
| n ≤ 1,000 | O(n²) | 2D DP, all pairs |
| n ≤ 100,000 | O(n log n) | sorting, heap, binary search |
| n ≤ 1,000,000 | O(n) | single pass, hash map, two pointers |
| n > 10,000,000 | O(log n) or O(1) | binary search, formula |

**Teach students to read the constraints first, every time.** It is free information and most candidates ignore it.

---

## 3. Python operation costs

| Operation | `list` | `dict` / `set` | `deque` | `heapq` |
|---|---|---|---|---|
| Index / key access | O(1) | O(1) avg | O(1) at ends | — |
| **Membership `in`** | **O(n)** | **O(1) avg** | O(n) | O(n) |
| Append / add | O(1) amortised | O(1) avg | O(1) | O(log n) push |
| **`insert(0,x)` / `pop(0)`** | **O(n)** | — | **O(1)** | — |
| Pop from end | O(1) | — | O(1) | O(log n) |
| Delete by value | O(n) | O(1) avg | O(n) | O(n) |
| Peek minimum | O(n) | — | — | **O(1)** |
| Iterate | O(n) | O(n) | O(n) | O(n) |
| Length | O(1) | O(1) | O(1) | O(1) |
| `sorted()` / `.sort()` | O(n log n) | — | — | — |
| `min` / `max` / `sum` | O(n) | O(n) | O(n) | — |
| Slice `a[i:j]` | O(j−i) | — | — | — |
| Copy | O(n) | O(n) | O(n) | O(n) |
| `heapify(list)` | — | — | — | **O(n)** |

**The two lines students must internalise:** `x in list` is O(n) while `x in set` is O(1) — and `list.pop(0)` is O(n) while `deque.popleft()` is O(1). Nearly every accidental O(n²) in this course comes from one of these two.

## 4. JavaScript operation costs

| Operation | `Array` | `Map` / `Set` |
|---|---|---|
| Index access | O(1) | — |
| `get` / `set` / `has` | — | O(1) avg |
| `push` / `pop` | O(1) amortised | — |
| **`shift` / `unshift`** | **O(n)** | — |
| `includes` / `indexOf` | O(n) | — |
| `splice` | O(n) | — |
| `sort` | O(n log n) | — |
| `slice` / `concat` / spread | O(n) | — |

**`shift()` used as a BFS dequeue silently turns O(V+E) into O(V²).** Use an array with a head index.

---

## 5. Algorithm complexities

| Algorithm | Time | Space | Notes |
|---|---|---|---|
| Linear scan | O(n) | O(1) | |
| Binary search | O(log n) | O(1) | needs a monotonic predicate |
| Binary search on the answer | O(n log(range)) | O(1) | feasibility check must be monotonic |
| Merge sort | O(n log n) | O(n) | stable; the right choice for linked lists |
| Quicksort | O(n log n) avg, **O(n²) worst** | O(log n) | not stable |
| Quickselect | **O(n) avg**, O(n²) worst | O(1) | k-th element without full sorting |
| Heap push / pop | O(log n) | O(1) | |
| Build heap from a list | **O(n)** | O(1) | not O(n log n) |
| Top-K with a size-k heap | **O(n log k)** | O(k) | beats sorting when k ≪ n |
| Sliding window | O(n) | O(k) | amortised — each element enters and leaves once |
| Monotonic stack | O(n) | O(n) | amortised — each index pushed once, popped once |
| Two pointers | O(n) | O(1) | usually after an O(n log n) sort |
| Prefix sums | O(n) build, O(1) query | O(n) | |
| Tree DFS / BFS | O(n) | O(h) / O(w) | h = height, w = max width |
| BST search (balanced) | O(log n) | O(1) | **O(n) if degenerate** |
| Trie insert / search | O(L) | O(total chars) | independent of the number of words |
| Graph DFS / BFS | O(V + E) | O(V) | |
| Topological sort (Kahn) | O(V + E) | O(V) | |
| Union-Find (compressed) | ~O(1) amortised | O(V) | inverse Ackermann; under 5 for any real input |
| Dijkstra (binary heap) | O((V + E) log V) | O(V) | non-negative weights only |
| DP | O(states × transitions) | O(states) | often reducible to one row |
| Backtracking | exponential | O(depth) | pruning changes the constant, not the class |

---

## 6. Recursion shapes → complexity

Read the complexity off the recursion tree by asking two questions: **how many calls per level, and how much does the input shrink?**

| Shape | Time | Example |
|---|---|---|
| One call, n−1 | O(n) | factorial |
| One call, n/2 | O(log n) | binary search, fast power |
| Two calls, n−1 | O(2ⁿ) | naive Fibonacci |
| Two calls, n/2, O(1) work | O(n) | tree traversal |
| Two calls, n/2, O(n) merge | O(n log n) | merge sort |
| n calls, n−1 | O(n!) | permutations |

**Space is the maximum recursion depth, not the number of calls.** Naive `fib(n)` makes 2ⁿ calls but uses only O(n) stack, because the tree is explored depth-first. This distinction is asked often and missed often.

---

## 7. Things students get wrong

| Claim | Reality |
|---|---|
| "Two sequential loops are O(n²)" | O(n) — sequential loops **add**, nested loops multiply |
| "A nested loop is always O(n²)" | Not if the inner bound is constant |
| "The `while` inside a `for` makes it O(n²)" | Not for sliding windows or monotonic stacks — use the amortised argument |
| "I used a hash map, so it's O(n)" | Not if you also sort inside the loop |
| "Sorting makes it O(n log n) overall" | Only if nothing dominates it — 3Sum sorts, and is still O(n²) |
| "Heapify is O(n log n)" | It is **O(n)** |
| "A BST is O(log n)" | Only if balanced; degenerate is O(n) |
| "Recursion uses O(number of calls) space" | It uses O(max depth) |
| "O(1) space" while allocating an output array | Output space is usually excluded — but **say which convention you are using** |
| "Big-O is how fast it runs" | It is how the cost *grows*. Constants can dominate at small n |

---

## 8. Amortised analysis — the four sightings in this course

The same argument appears four times. By the fourth, students should recognise it immediately.

| Week | Where | The argument |
|---|---|---|
| 1 | `list.append` | Occasional reallocation and copy, but spread across all appends the cost is constant |
| 3 | Sliding window | `left` only moves forward, so it advances at most n times in total |
| 6 | Monotonic stack | Each index is pushed once and popped at most once → at most 2n operations |
| 6 | Queue from two stacks | Each element moves between the stacks at most once |

> **The shape of the argument, in one sentence:** *"This inner loop looks unbounded, but the total work it can ever do across the whole run is bounded by n — so the amortised cost per step is O(1)."*
