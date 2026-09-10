# Week 10 — Trees II · Heaps · Tries · Optional Practice

**Phase II** · Week band: **Medium/Hard** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Three structures in three days, and heaps are the one that will keep appearing — in Dijkstra next week, and in any problem with "top K" or "k closest" in it. If you only use one track this week, use Wednesday's.

---

## Session 1 (Tuesday) — Tree Construction & Serialization

> Optional. Not graded. Skip freely.

### Check your understanding
1. Serialising a tree: why does preorder-with-null-markers reconstruct uniquely while plain preorder does not?
2. You are given inorder and preorder. Which one tells you the root, and which one tells you where to split?
3. Why can you *not* rebuild a binary tree from inorder alone?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 637 Average of Levels in Binary Tree | Easy | 20 min |
| LC 872 Leaf-Similar Trees | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 117 Populating Next Right Pointers in Each Node II | Medium | 40 min |
| LC 1032 Stream of Characters | **Hard** | 55 min |

LC 1032 is a trie plus a stream, and it belongs to Thursday as much as today — worth keeping in mind when you get there.

### 3. Revision — Week 9 (Trees I) · Week 7 (Recursion) · Week 4 (Strings & Matrices)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1528 Shuffle String | Easy | 25 min |
| LC 107 Binary Tree Level Order Traversal II | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why do you need two traversals to reconstruct the tree?"** — A good answer explains that one gives structure and the other gives ordering, and that preorder+postorder is *not* sufficient for a general binary tree. Knowing which pairs work is the actual content here.
2. **"What is the complexity of your reconstruction, and can you improve it?"** — A good answer spots the repeated linear search for the root in the inorder array — O(n²) — and fixes it with a value-to-index hash map for O(n). That is Week 1's hash map earning its keep.

---

## Session 2 (Wednesday) — Heaps & Priority Queues

> Optional. Not graded. Skip freely.

### Check your understanding
1. A heap gives you the minimum in O(1) but not a sorted list. What operation is O(log n), and why is that the right trade for top-K?
2. For "K largest", do you use a min-heap or a max-heap, and of what size? Justify the choice.
3. Python's `heapq` is a min-heap only. Give two ways to get max-heap behaviour.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1464 Maximum Product of Two Elements in an Array | Easy | 20 min |
| LC 506 Relative Ranks | Easy | 25 min |

Both are heap problems only if you choose to solve them that way — try each with a sort first, then with a heap, and compare the complexities. That comparison is the lesson.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 658 Find K Closest Elements | Medium | 40 min |
| LC 2402 Meeting Rooms III | **Hard** | 55 min |

### 3. Revision — Week 9 (Trees I) · Week 7 (Recursion) · Week 4 (Strings & Matrices)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 766 Toeplitz Matrix | Easy | 25 min |
| LC 378 Kth Smallest Element in a Sorted Matrix | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why a heap rather than sorting?"** — A good answer gives O(n log k) versus O(n log n) and notes that when k is small the difference is large, and when k approaches n sorting is simpler and just as good. Naming the crossover is the strong part.
2. **"Could you do it in O(n)?"** — A good answer reaches for quickselect from Week 7, gives O(n) average and O(n²) worst case, and says why an interviewer might still prefer the heap: predictable performance and less code to get wrong.

---

## Session 3 (Thursday) — Tries

> Optional. Not graded. Skip freely.

### Check your understanding
1. A trie node holds what, exactly? Why is a hash map of children usually better than a 26-slot array in practice?
2. What does the `is_end` flag exist for — give a concrete bug that occurs without it.
3. Inserting n words of average length k: what is the time and space complexity, and what is the trie buying you over a set of strings?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 386 Lexicographical Numbers | Medium | 35 min |
| LC 3043 Find the Length of the Longest Common Prefix | Medium | 40 min |

Easy trie problems barely exist — every one is in the standard set — so this is a gentle **Medium** pair. Both are prefix problems solvable without a trie; do them both ways.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1233 Remove Sub-Folders from the Filesystem | Medium | 40 min |
| LC 336 Palindrome Pairs | **Hard** | 55 min |

LC 336 is a Hard that combines a trie with palindrome checking, and it is the best argument for tries being more than autocomplete.

### 3. Revision — Week 9 (Trees I) · Week 7 (Recursion) · Week 4 (Strings & Matrices)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 3110 Score of a String | Easy | 25 min |
| LC 99 Recover Binary Search Tree | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why a trie rather than a hash set of prefixes?"** — A good answer compares space — shared prefixes stored once — and notes that a trie answers "does any word start with this?" in O(k) while a set of all prefixes costs O(nk) space to build. Prefix queries are the whole reason the structure exists.
2. **"How would you handle deletion?"** — A good answer clears the end flag and only prunes nodes with no remaining children, and spots that pruning eagerly would break other words sharing the prefix.

---

## Session 4 (Friday) — ARENA: Contest 6

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Heap, trie, or tree? Give the trigger phrase in a problem statement that points at each.
2. From memory, in a blank file: a trie with insert and search, then top-K with a heap. Both.
3. Next week is graphs, and Dijkstra in Week 12 is BFS plus a heap. Which of this week's three structures do you most need solid before then?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 3264 Final Array State After K Multiplication Operations I | Easy | 20 min |
| LC 2558 Take Gifts From the Richest Pile | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 373 Find K Pairs with Smallest Sums | Medium | 40 min |
| LC 632 Smallest Range Covering Elements from K Lists | **Hard** | 55 min |

LC 632 is a heap over multiple sorted lists — the same shape as merging k sorted lists, and a direct preview of how Dijkstra picks its next node.

### 3. Revision — Week 9 (Trees I) · Week 7 (Recursion) · Week 4 (Strings & Matrices)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2114 Maximum Number of Words Found in Sentences | Easy | 25 min |
| LC 109 Convert Sorted List to Binary Search Tree | Medium | 35 min |

### 4. Interview follow-ups
1. **"Your solution uses a heap of size k. Why not n?"** — A good answer ties the heap size to what the question asks for and notes that keeping only k elements is what turns O(n log n) into O(n log k). Being able to say which elements can be discarded, and why, is the insight.
2. **"Could you solve this with a sort instead? Would you?"** — A good answer says yes and then argues on clarity: in an interview, the simpler solution stated with its complexity, followed by "and here is how I'd improve it if the input were large", scores better than a heap written badly.
