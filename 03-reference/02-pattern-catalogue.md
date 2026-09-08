# Pattern Catalogue

The ~29 patterns this course delivers. **Nobody memorises 300 solutions.** They learn these, each with a *trigger signal* — the phrase in a problem statement that should make the pattern fire.

Students should be able to read any problem and name the pattern within 3 minutes. That is the skill Week 16's recognition drill assesses.

Full code for every template: `03-code-templates-python.md`.

---

## Phase I patterns (Weeks 1–6)

### 1. Hash map lookup / complement
**Trigger:** "find two elements that…" · "has this been seen before?" · "does X exist in the collection?"
**Idea:** trade space for time. An O(n²) scan-for-a-partner becomes O(n) by remembering what you've seen.
**Complexity:** O(n) time, O(n) space
**Canonical:** LC 1 Two Sum · LC 217 Contains Duplicate · LC 219 Contains Duplicate II
**Watch for:** storing the *index* vs the *value* — decide which the problem needs before writing.

### 2. Frequency counting & grouping
**Trigger:** "anagram" · "most frequent" · "group by" · "appears exactly k times"
**Idea:** a `Counter` (or a 26-length array for lowercase letters) reduces comparison to equality of counts.
**Complexity:** O(n) time, O(k) space where k = alphabet or distinct values
**Canonical:** LC 242 Valid Anagram · LC 49 Group Anagrams · LC 347 Top K Frequent Elements
**Watch for:** the canonical key when grouping — sorted string, or a 26-tuple of counts (the latter is O(n) not O(n log n)).

### 3. Prefix sums & difference arrays
**Trigger:** "sum of a subarray" · "range query" · "how many subarrays sum to k" · repeated range updates
**Idea:** precompute cumulative sums so any range is one subtraction. Difference arrays invert this for range *updates*.
**Complexity:** O(n) build, O(1) per query
**Canonical:** LC 303 Range Sum Query · LC 560 Subarray Sum Equals K · LC 238 Product of Array Except Self
**Watch for:** LC 560 combines prefix sums *with* a hash map — the most important composite in Phase I.

### 4. Kadane / running optimum
**Trigger:** "maximum subarray" · "best contiguous run"
**Idea:** at each element decide — extend the current run, or start fresh here?
**Complexity:** O(n) time, O(1) space
**Canonical:** LC 53 Maximum Subarray · LC 152 Maximum Product Subarray · LC 121 Best Time to Buy and Sell Stock
**Watch for:** this is DP in disguise. Say so in Week 13 — it makes DP feel familiar rather than new.

### 5. Two pointers — opposite ends
**Trigger:** **sorted** array + "find a pair/triplet" · "palindrome" · "container/area between two positions"
**Idea:** start at both ends, move the pointer that can improve the answer. The sortedness tells you which one.
**Complexity:** O(n) after sorting
**Canonical:** LC 167 Two Sum II · LC 15 3Sum · LC 11 Container With Most Water · LC 125 Valid Palindrome
**Watch for:** in 3Sum, skipping duplicates is most of the difficulty, not the two-pointer scan.

### 6. Two pointers — same direction (read/write)
**Trigger:** "remove in place" · "move all X to the end" · "O(1) extra space" on an array
**Idea:** a *read* pointer scans; a *write* pointer marks where the next kept element goes.
**Complexity:** O(n) time, O(1) space
**Canonical:** LC 26 Remove Duplicates from Sorted Array · LC 283 Move Zeroes · LC 75 Sort Colors
**Watch for:** LC 75 (Dutch national flag) needs three pointers — the natural extension.

### 7. Fast & slow pointers
**Trigger:** "cycle" · "middle of the list" · "nth from the end" · linked list + O(1) space
**Idea:** two pointers at different speeds. They meet iff there is a cycle; when fast hits the end, slow is at the middle.
**Complexity:** O(n) time, O(1) space
**Canonical:** LC 141 Linked List Cycle · LC 876 Middle of the Linked List · LC 142 Cycle II · LC 287 Find the Duplicate Number
**Watch for:** LC 287 applies this to an *array* — a favourite interview trick.

### 8. Sliding window
**Trigger:** "contiguous subarray/substring" + "longest / shortest / max / at most k"
**Idea:** expand right; while the window is invalid, shrink from the left. Every element enters and leaves once.
**Complexity:** O(n) time
**Canonical:** LC 3 Longest Substring Without Repeating · LC 424 Longest Repeating Character Replacement · LC 76 Minimum Window Substring · LC 209 Minimum Size Subarray Sum
**Watch for:** the two shapes — *fixed* size (window length given) and *variable* (shrink while invalid). Teach the variable one as the default.

### 9. In-place array manipulation
**Trigger:** "in place" · "O(1) extra space" · "rotate" · "without allocating"
**Idea:** reversal tricks, swapping, index arithmetic, or encoding two values in one slot.
**Canonical:** LC 189 Rotate Array (triple reversal) · LC 48 Rotate Image (transpose + reverse) · LC 41 First Missing Positive
**Watch for:** LC 41 uses the array itself as a hash map — the highest-yield idea in this family.

### 10. Matrix / grid traversal
**Trigger:** 2D input · "spiral" · "rotate" · "islands" · "neighbours"
**Idea:** boundary-shrinking for spiral; transpose+reverse for rotation; a `(row, col)` grid is a graph whose neighbours are the 4 (or 8) adjacent cells.
**Canonical:** LC 54 Spiral Matrix · LC 48 Rotate Image · LC 73 Set Matrix Zeroes · LC 200 Number of Islands
**Watch for:** "a grid is a graph" is the bridge to Week 11 — plant it here.

### 11. Linked-list pointer surgery
**Trigger:** any linked list problem
**Idea:** a **dummy head node** removes almost every edge case. Reversal is the one template to have in your fingers.
**Canonical:** LC 206 Reverse Linked List · LC 21 Merge Two Sorted Lists · LC 19 Remove Nth From End · LC 143 Reorder List · LC 138 Copy List with Random Pointer
**Watch for:** always draw the pointers before writing. Always.

### 12. Monotonic stack / deque
**Trigger:** "next greater/smaller element" · "previous smaller" · "largest rectangle" · sliding window max
**Idea:** keep the stack monotonic. Popping is where the answer gets computed — each element is pushed and popped at most once.
**Complexity:** O(n) despite the nested-looking loop
**Canonical:** LC 496 Next Greater Element I · LC 739 Daily Temperatures · LC 84 Largest Rectangle in Histogram · LC 239 Sliding Window Maximum
**Watch for:** proving O(n) amortised is the lesson here, not the code.

---

## Phase II patterns (Weeks 7–13)

### 13. Recursion
**Trigger:** the problem is naturally defined in terms of itself · trees · "all combinations"
**The three questions**, asked of every recursive function: *What is the base case? What is the smaller subproblem? How do I combine the results?*
**Canonical:** factorial · fib · LC 344 Reverse String · LC 21 Merge Two Sorted Lists (recursive) · LC 50 Pow(x, n)
**Watch for:** the leap of faith — trust that the recursive call returns the right answer for the smaller input. This is the whole wall.

### 14. Divide & conquer
**Trigger:** "sort" · "the answer for the whole is combinable from the answers for the halves"
**Idea:** split, solve both halves recursively, merge. The merge step is where the work lives.
**Complexity:** typically O(n log n)
**Canonical:** merge sort · quicksort partition · LC 912 Sort an Array · LC 23 Merge k Sorted Lists · LC 53 Maximum Subarray (D&C variant)

### 15a. Binary search on a sorted array
**Trigger:** sorted input + "find" · "O(log n) required"
**Idea:** halve the search space each step. **Use one template** — `lo`, `hi`, and a loop invariant you can state — and never improvise the boundaries.
**Canonical:** LC 704 Binary Search · LC 35 Search Insert Position · LC 33 Search in Rotated Sorted Array · LC 153 Find Minimum in Rotated Sorted Array
**Watch for:** lower-bound vs upper-bound. Learn one, derive the other.

### 15b. Binary search on the answer space
**Trigger:** "minimum X such that…" · "maximum capacity/speed/size that still works" · a monotonic feasibility check
**Idea:** you are not searching the array — you are searching the *range of possible answers*, and testing feasibility at each guess.
**Complexity:** O(n log(range))
**Canonical:** LC 875 Koko Eating Bananas · LC 1011 Capacity To Ship Packages · LC 410 Split Array Largest Sum · LC 4 Median of Two Sorted Arrays
**Watch for:** the highest-value pattern most self-taught candidates never learn. Ask: *"If I guess X, can I check feasibility in O(n)? Is feasibility monotonic in X?"* If yes to both, binary search the answer.

### 16. Sort-then-scan · interval merging
**Trigger:** "intervals" · "meeting rooms" · "overlapping" · "schedule"
**Idea:** sort by start (or end), then a single linear scan. Choosing the sort key *is* the problem.
**Complexity:** O(n log n)
**Canonical:** LC 56 Merge Intervals · LC 57 Insert Interval · LC 435 Non-overlapping Intervals · LC 252/253 Meeting Rooms I & II
**Watch for:** sort by **end** for "maximum non-overlapping" (a greedy classic); by **start** for merging.

### 17. Tree DFS & BFS
**Trigger:** any binary tree problem
**Idea:** DFS = recursion (pre/in/post order differ only in *when* you process the node). BFS = a queue, level by level.
**Complexity:** O(n) time, O(h) space for DFS, O(w) for BFS
**Canonical:** LC 104 Maximum Depth · LC 102 Level Order Traversal · LC 226 Invert Binary Tree · LC 543 Diameter · LC 124 Max Path Sum
**Watch for:** the "return one thing, track another" idiom (diameter, max path sum) — the hardest idea in tree problems.

### 18. BST invariant exploitation
**Trigger:** "binary **search** tree"
**Idea:** left < node < right, so **inorder traversal is sorted** and search prunes half the tree.
**Canonical:** LC 98 Validate BST · LC 230 Kth Smallest · LC 235 LCA of a BST · LC 700 Search in a BST
**Watch for:** LC 98 requires passing down a valid *range*, not just comparing with the parent. Almost everyone gets this wrong first.

### 19. Heap / top-K
**Trigger:** "k largest/smallest" · "median of a stream" · "merge k sorted" · "closest k"
**Idea:** a heap gives O(log n) insert and O(1) peek at the extreme. For top-K keep a heap of size k — O(n log k), better than sorting.
**Canonical:** LC 215 Kth Largest Element · LC 347 Top K Frequent · LC 23 Merge k Sorted Lists · LC 295 Find Median from Data Stream
**Watch for:** Python's `heapq` is a **min-heap**; negate values for a max-heap. JS has no built-in heap — students must write one.

### 20. Trie / prefix tree
**Trigger:** "prefix" · "autocomplete" · "dictionary of words" · "search word with wildcards"
**Idea:** a tree where each edge is a character; shared prefixes share a path.
**Complexity:** O(L) insert and search, L = word length
**Canonical:** LC 208 Implement Trie · LC 211 Design Add and Search Words · LC 212 Word Search II
**Watch for:** LC 212 = trie + backtracking on a grid, and is the payoff problem for the whole term.

### 21. Graph traversal & connectivity
**Trigger:** "connected" · "path exists" · "islands" · "regions" · anything relational
**Idea:** DFS or BFS over an adjacency list, with a `visited` set. A grid is a graph. **BFS gives shortest path in unweighted graphs; DFS does not.**
**Canonical:** LC 200 Number of Islands · LC 133 Clone Graph · LC 417 Pacific Atlantic · LC 994 Rotting Oranges (multi-source BFS)
**Watch for:** multi-source BFS — seed the queue with *all* starts at once. Non-obvious and very common.

### 22. Topological sort
**Trigger:** "prerequisites" · "build order" · "course schedule" · "is there a cycle in a directed graph?"
**Idea:** Kahn's algorithm — repeatedly remove nodes with in-degree 0. If nodes remain, there is a cycle.
**Complexity:** O(V + E)
**Canonical:** LC 207 Course Schedule · LC 210 Course Schedule II · LC 269 Alien Dictionary
**Watch for:** cycle detection comes free. That is often the actual question.

### 23. Union-Find (DSU)
**Trigger:** "connected components" · "are these two in the same group?" · edges arriving dynamically
**Idea:** each set has a representative. `find` with path compression, `union` by rank/size. Near-O(1) amortised.
**Canonical:** LC 547 Number of Provinces · LC 684 Redundant Connection · LC 323 Connected Components
**Watch for:** when both DSU and DFS work, DSU wins if edges arrive incrementally; DFS wins if the graph is static and you need the traversal.

### 24. Dijkstra / weighted shortest path
**Trigger:** "shortest path" + **weighted** edges, non-negative
**Idea:** BFS with a priority queue instead of a plain queue. Always expand the nearest unvisited node.
**Complexity:** O((V + E) log V)
**Canonical:** LC 743 Network Delay Time · LC 787 Cheapest Flights Within K Stops · LC 1631 Path With Minimum Effort
**Watch for:** if all weights are equal, plain BFS is correct and simpler. Check before reaching for a heap.

### 25. Greedy & the exchange argument
**Trigger:** "minimum number of…" · "maximum you can…" and a locally-optimal choice that seems safe
**Idea:** take the locally best option. **The hard part is proving it is safe** — the exchange argument: show any optimal solution can be transformed into the greedy one without getting worse.
**Canonical:** LC 55 Jump Game · LC 45 Jump Game II · LC 134 Gas Station · LC 621 Task Scheduler · LC 435 Non-overlapping Intervals
**Watch for:** the discipline is knowing **when greedy fails** and DP is needed. Coin Change with arbitrary denominations is the canonical counterexample — teach it explicitly.

### 26. Dynamic programming
**Trigger:** "how many ways" · "min/max cost" · overlapping subproblems · a brute-force recursion recomputing the same states
**The procedure**, in this order, every time:
1. **Define the state.** What does `dp[i]` *mean*, in one English sentence? (This is where most failures happen.)
2. **Write the recurrence.** How does `dp[i]` follow from smaller states?
3. **Base cases.**
4. **Order of computation.**
5. *Then* optimise space if you can.

**Always start from memoized recursion, then convert to a table.** Never start with the table.

**The six families:**

| Family | Recognition | Canonical |
|---|---|---|
| Linear / 1D | answer at `i` depends on a few previous | LC 70 Climbing Stairs · LC 198 House Robber · LC 91 Decode Ways |
| Grid / 2D | movement on a grid | LC 62 Unique Paths · LC 64 Minimum Path Sum |
| Subsequence | "longest/count of subsequence" | LC 300 LIS · LC 1143 LCS |
| Knapsack | choose items under a capacity | LC 322 Coin Change · LC 416 Partition Equal Subset Sum |
| String / two-sequence | two strings, `dp[i][j]` | LC 72 Edit Distance · LC 516 Longest Palindromic Subsequence |
| State machine | hold/sell/cooldown states | LC 121/122/309/188 Stock series |

---

## Phase III patterns (Weeks 14–16)

### 27. Backtracking
**Trigger:** "all permutations/combinations/subsets" · "N-Queens" · "generate every valid…"
**Idea:** DFS over a decision tree — **choose, explore, un-choose**. The un-choose is what makes it backtracking rather than plain DFS.
**Complexity:** exponential; pruning is what makes it tractable
**Canonical:** LC 78 Subsets · LC 46 Permutations · LC 39 Combination Sum · LC 51 N-Queens · LC 79 Word Search
**Watch for:** one template covers all of these. Learn the template, then vary the choice set and the pruning condition.

### 28. Bit manipulation
**Trigger:** "without extra space" on integers · "appears once while others appear twice" · subsets of a small set · "count bits"
**Key facts:** `x ^ x == 0` · `x & (x-1)` clears the lowest set bit · `x & -x` isolates it · a bitmask enumerates subsets of a set of ≤ 20 elements
**Canonical:** LC 136 Single Number · LC 191 Number of 1 Bits · LC 338 Counting Bits · LC 371 Sum of Two Integers
**Watch for:** in Python, integers are arbitrary-precision and negatives have no fixed width — masking with `0xFFFFFFFF` is needed for problems assuming 32-bit ints.

### 29. Data-structure design
**Trigger:** "design a class that supports X in O(1)"
**Idea:** almost always **combine two structures** so each operation is fast in one of them — hash map + doubly linked list, hash map + array, two heaps.
**Canonical:** LC 146 LRU Cache · LC 155 Min Stack · LC 380 Insert Delete GetRandom O(1) · LC 295 Median from Data Stream
**Watch for:** the interview question is really "what does each operation need, and which structure gives it?" Reason from the required complexities backwards.

---

## The discrimination table

By Week 16 students must distinguish patterns that *look* alike. These pairs are drilled deliberately (see `01-instructor/04-spaced-revision-system.md`, §5):

| These look alike | Tell them apart by |
|---|---|
| Sliding window vs. two pointers | Window = a contiguous range with a property; two pointers = converging on a target |
| BFS vs. level-order traversal | They are the same algorithm — one on a graph, one on a tree |
| DFS vs. backtracking | Backtracking undoes its choice on the way out |
| Greedy vs. DP | Greedy commits and never reconsiders; DP keeps all options. If a local choice can be regretted, use DP |
| Binary search on array vs. on answer | What is the search space — the input, or the range of possible outputs? |
| Union-Find vs. DFS components | DSU for incremental edges and repeated connectivity queries; DFS for a static graph |
| Memoization vs. tabulation | Same recurrence, opposite direction |
