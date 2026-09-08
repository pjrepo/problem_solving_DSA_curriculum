# Canonical Code Templates (Python)

**Teach exactly these forms.** Students see one consistent shape all term, which is what makes them reproducible under interview pressure.

Every template below is the version used in the session it belongs to. If you improvise a variant in class, students will end up with two half-remembered shapes and neither will survive a contest.

---

## 1. Sliding window — variable (Week 3)

```python
def longest_valid_window(s):
    state = {}                     # whatever defines validity
    left = 0
    best = 0
    for right, ch in enumerate(s):
        # 1. EXPAND
        state[ch] = state.get(ch, 0) + 1

        # 2. SHRINK while INVALID
        while state[ch] > 1:
            state[s[left]] -= 1
            left += 1

        # 3. RECORD (window is now valid)
        best = max(best, right - left + 1)
    return best
```

**Minimising variant** — shrink while *valid*, record *before* breaking validity:

```python
        while is_valid(state):
            best = min(best, right - left + 1)
            state[s[left]] -= 1
            left += 1
```

O(n): `left` only moves forward, so each element enters and leaves once.

---

## 2. Two pointers — opposite ends (Week 3)

```python
def two_sum_sorted(a, target):
    lo, hi = 0, len(a) - 1
    while lo < hi:
        s = a[lo] + a[hi]
        if s == target:  return [lo, hi]
        elif s < target: lo += 1        # only increasing the sum can help
        else:            hi -= 1
    return []
```

## 3. Two pointers — read/write (Week 3)

```python
def remove_duplicates(nums):
    write = 1                            # frontier of the kept prefix
    for read in range(1, len(nums)):
        if nums[read] != nums[write - 1]:
            nums[write] = nums[read]
            write += 1
    return write
```

**Invariant:** everything before `write` is finished and correct; everything from `read` on is unexamined.

---

## 4. Linked list — reversal (Week 5)

```python
def reverse_list(head):
    prev, cur = None, head
    while cur:
        nxt = cur.next        # 1. SAVE
        cur.next = prev       # 2. FLIP
        prev = cur            # 3. advance prev
        cur = nxt             # 4. advance cur
    return prev
```

**Dummy head** — use it whenever the first node might change:
```python
dummy = ListNode(0, head)
prev = dummy
# ... work ...
return dummy.next
```

**Fast & slow:**
```python
slow = fast = head
while fast and fast.next:            # both checks required
    slow, fast = slow.next, fast.next.next
    if slow is fast: return True     # cycle
```

---

## 5. Monotonic stack (Week 6)

```python
def next_greater(nums):
    ans = [-1] * len(nums)
    stack = []                                  # indices, values decreasing
    for i, v in enumerate(nums):
        while stack and nums[stack[-1]] < v:
            ans[stack.pop()] = v                # v resolves that index
        stack.append(i)
    return ans
```

**Invariant:** the stack holds indices, in decreasing value order, still awaiting an answer.
**O(n)** — each index is pushed once and popped at most once.

---

## 6. Recursion — the three questions (Week 7)

```python
def solve(problem):
    if is_base_case(problem):        # 1. BASE CASE
        return base_answer
    smaller = reduce(problem)        # 2. SMALLER SUBPROBLEM
    # ASSUME solve(smaller) returns the correct answer
    return combine(solve(smaller))   # 3. COMBINE
```

## 7. Merge sort (Week 7)

```python
def merge_sort(a):
    if len(a) <= 1: return a                    # base case: <= 1, not == 0
    mid = len(a) // 2
    return merge(merge_sort(a[:mid]), merge_sort(a[mid:]))

def merge(x, y):
    out, i, j = [], 0, 0
    while i < len(x) and j < len(y):
        if x[i] <= y[j]: out.append(x[i]); i += 1
        else:            out.append(y[j]); j += 1
    out.extend(x[i:]); out.extend(y[j:])        # one side is already empty
    return out
```

---

## 8. Binary search — array (Week 8)

```python
def binary_search(a, target):
    lo, hi = 0, len(a) - 1               # INVARIANT: target, if present, is in a[lo..hi]
    while lo <= hi:                      # <= : a single-element range must be checked
        mid = lo + (hi - lo) // 2
        if a[mid] == target:  return mid
        elif a[mid] < target: lo = mid + 1     # mid+1, never mid
        else:                 hi = mid - 1
    return -1
```

## 9. Binary search — lower bound (Week 8)

```python
def lower_bound(a, target):
    lo, hi = 0, len(a)                   # half-open [lo, hi)
    while lo < hi:                       # strict <
        mid = (lo + hi) // 2
        if a[mid] < target: lo = mid + 1
        else:               hi = mid     # mid may be the answer — keep it
    return lo                            # may be len(a)
```

`upper_bound(t) == lower_bound(t + 1)` for integers. Learn one, derive the other.

## 10. Binary search — the answer space (Week 8)

```python
def min_feasible(lo, hi, feasible):
    """Smallest x in [lo, hi] with feasible(x) True. Requires monotonicity."""
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid): hi = mid
        else:             lo = mid + 1
    return lo
```

**Before using it, answer both:** can I check feasibility quickly? Is feasibility monotonic in x?

---

## 11. Tree traversals (Week 9)

```python
def dfs(node):
    if not node: return              # base case is almost always line 1
    # PRE-order:  process here
    dfs(node.left)
    # IN-order:   process here  (sorted output on a BST)
    dfs(node.right)
    # POST-order: process here (needs children's results first)
```

**BFS / level order:**
```python
def level_order(root):
    if not root: return []
    out, q = [], deque([root])
    while q:
        level = []
        for _ in range(len(q)):          # freeze the level size FIRST
            node = q.popleft()
            level.append(node.val)
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)
        out.append(level)
    return out
```

**"Return one thing, track another"** (diameter, max path sum):
```python
def diameter(root):
    best = 0
    def height(node):
        nonlocal best
        if not node: return 0
        L, R = height(node.left), height(node.right)
        best = max(best, L + R)          # the answer THROUGH this node
        return 1 + max(L, R)             # what the PARENT needs
    height(root)
    return best
```

**BST validation — pass the range down:**
```python
def is_valid_bst(root):
    def check(node, lo, hi):
        if not node: return True
        if not (lo < node.val < hi): return False
        return check(node.left, lo, node.val) and check(node.right, node.val, hi)
    return check(root, float('-inf'), float('inf'))
```

---

## 12. Heap (Week 10)

```python
import heapq

h = []
heapq.heappush(h, x)
smallest = heapq.heappop(h)
heapq.heappush(h, -x); largest = -heapq.heappop(h)     # max-heap by negation
heapq.heappush(h, (priority, tiebreak_counter, item))  # counter avoids comparing payloads

def top_k_largest(nums, k):
    h = []
    for v in nums:
        heapq.heappush(h, v)
        if len(h) > k: heapq.heappop(h)    # evict the weakest survivor
    return h                               # h[0] is the k-th largest
```

## 13. Trie (Week 10)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False               # without this, every prefix reads as a word

class Trie:
    def __init__(self): self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            node = node.children.setdefault(ch, TrieNode())
        node.is_end = True

    def _walk(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return None
            node = node.children[ch]
        return node

    def search(self, word):
        n = self._walk(word);  return n is not None and n.is_end

    def startsWith(self, prefix):
        return self._walk(prefix) is not None
```

---

## 14. Graph DFS & BFS (Week 11)

```python
def dfs(node, graph, visited):
    if node in visited: return
    visited.add(node)                    # the ONLY new line vs a tree
    for nxt in graph[node]:
        dfs(nxt, graph, visited)

def bfs_shortest(start, graph):
    dist = {start: 0}
    q = deque([start])
    while q:
        node = q.popleft()
        for nxt in graph[node]:
            if nxt not in dist:
                dist[nxt] = dist[node] + 1
                q.append(nxt)            # mark visited at ENQUEUE, not dequeue
    return dist
```

**Grid neighbours:**
```python
DIRS = [(-1,0), (1,0), (0,-1), (0,1)]
for dr, dc in DIRS:
    nr, nc = r + dr, c + dc
    if 0 <= nr < rows and 0 <= nc < cols:
        ...
```

**Multi-source BFS** — seed with every source at once:
```python
q = deque(all_sources)
```

## 15. Topological sort — Kahn's (Week 11)

```python
def topo_sort(n, edges):
    graph, indeg = defaultdict(list), [0] * n
    for u, v in edges:                      # u before v
        graph[u].append(v); indeg[v] += 1

    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []
    while q:
        node = q.popleft(); order.append(node)
        for nxt in graph[node]:
            indeg[nxt] -= 1
            if indeg[nxt] == 0: q.append(nxt)

    return order if len(order) == n else []  # short => a cycle exists
```

---

## 16. Union-Find (Week 12)

```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
        self.count = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])   # path compression
        return self.parent[x]

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb: return False                        # already connected
        if self.size[ra] < self.size[rb]: ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        self.count -= 1
        return True
```

## 17. Dijkstra (Week 12)

```python
def dijkstra(graph, start):
    dist = {start: 0}
    pq = [(0, start)]
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist.get(node, float('inf')):
            continue                                     # stale entry
        for nxt, w in graph[node]:
            nd = d + w
            if nd < dist.get(nxt, float('inf')):
                dist[nxt] = nd
                heapq.heappush(pq, (nd, nxt))
    return dist
```

Non-negative weights only. If all weights are equal, use plain BFS.

---

## 18. Dynamic programming (Weeks 13–14)

**Always start here — memoized recursion:**
```python
def solve(...):
    memo = {}
    def f(state):
        if is_base(state): return base_value
        if state in memo:  return memo[state]
        memo[state] = combine(f(smaller) for smaller in transitions(state))
        return memo[state]
    return f(initial_state)
```

**Then, if needed, tabulate:**
```python
dp = [base] * (n + 1)          # dp[i] = <ONE ENGLISH SENTENCE>
for i in range(1, n + 1):
    dp[i] = recurrence(dp[i-1], dp[i-2], ...)
return dp[n]
```

**Knapsack — the loop direction is the variant:**
```python
for item in items:
    for c in range(capacity, item.w - 1, -1):     # 0/1: BACKWARD
        dp[c] = max(dp[c], dp[c - item.w] + item.v)

for item in items:
    for c in range(item.w, capacity + 1):         # UNBOUNDED: FORWARD
        dp[c] = max(dp[c], dp[c - item.w] + item.v)
```

**Two-sequence DP** — `(m+1) x (n+1)`, row/column 0 means the empty string:
```python
dp = [[0] * (n + 1) for _ in range(m + 1)]
for i in range(1, m + 1):
    for j in range(1, n + 1):
        if A[i-1] == B[j-1]: dp[i][j] = dp[i-1][j-1] + 1
        else:                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

**State machine:**
```python
hold, free = float('-inf'), 0
for p in prices:
    hold, free = max(hold, free - p), max(free, hold + p)   # simultaneous update
return free
```

---

## 19. Backtracking (Week 15)

```python
def backtrack(path, choices, result):
    if is_complete(path):
        result.append(path[:])           # COPY — path keeps mutating
        return
    for choice in choices:
        if not is_valid(choice, path):
            continue                     # PRUNE — this is where performance lives
        path.append(choice)              # CHOOSE
        backtrack(path, next_choices(choice), result)   # EXPLORE
        path.pop()                       # UN-CHOOSE
```

**Subsets / combinations** — pass a start index. **Permutations** — track a `used` array.
**Duplicates** — sort first, then `if i > start and nums[i] == nums[i-1]: continue`.

## 20. Bit manipulation (Week 15)

```python
x ^ x == 0            # a value XORed with itself vanishes
x & 1                 # is x odd
x >> 1                # divide by 2
x & (x - 1)           # clear the lowest set bit
x & -x                # isolate the lowest set bit
1 << i                # the i-th bit
mask | (1 << i)       # add item i to a subset mask
mask & (1 << i)       # is item i in the mask
```

**32-bit emulation in Python** (integers are arbitrary-precision):
```python
MASK = 0xFFFFFFFF
result &= MASK
if result > 0x7FFFFFFF:
    result = ~(result ^ MASK)
```

---

## 21. Design — combining two structures (Week 15)

The method: **read the required complexity per operation, pick a structure for each, then combine.**

| Need | Structure |
|---|---|
| O(1) lookup by key | hash map |
| O(1) insert/remove at a known position | doubly linked list |
| O(1) random access by index | array |
| O(log n) min/max | heap |

**LRU Cache** = hash map (key → **node reference**) + doubly linked list (recency order) + dummy head/tail.
**Insert/Delete/GetRandom O(1)** = array (for random) + hash map (value → index), deleting by swapping with the last element and popping.
