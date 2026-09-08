# Week 10 — Trees II, Heaps & Tries

**Phase II: Techniques** · Difficulty band: **Medium/Hard**

> Three related structures. Tree construction closes out Week 9; **heaps** answer "what is the k-th / most extreme thing?" in O(log n); **tries** answer "what shares this prefix?". All three are frequently asked, and all three are structures students must be able to *build*, not just use.
>
> This week also revisits three problems solved earlier with different tools — LC 215, LC 347 and LC 23 — deliberately. Seeing the same problem yield to a different structure is how students learn to *choose* rather than pattern-match.

---

## Exit criteria

- [ ] Reconstruct a binary tree from pre-order + in-order, and explain why that pair suffices while pre+post does not
- [ ] Explain the heap property, and why it gives O(1) peek and O(log n) push/pop
- [ ] Explain why top-K with a size-k heap is **O(n log k)**, and when that beats sorting
- [ ] Use a max-heap in Python (negate values) and know JavaScript has none built in
- [ ] Solve the median-of-a-stream problem with two heaps
- [ ] Implement a trie with insert, search and startsWith
- [ ] Explain why a trie plus backtracking beats searching a grid once per word

---

## Session 1 (Tuesday) — Tree Construction & Serialization

### Weekend-set debrief (0:00–0:15)
Debrief **LC 124** — check the editorials for both halves of the observation (two different quantities, and clamping negatives). Then **LC 437**: ask who recognised LC 560 running on a tree. If nobody did, show it now — it is one of the most striking cross-week connections in the course.

### Concept spine (0:15–0:45)

**Part A — what a traversal tells you.**

Put `preorder = [3,9,20,15,7]` and `inorder = [9,3,15,20,7]` on the board.

*"What does pre-order's first element tell you?"* → the root, 3.
*"Now look at in-order. Where is 3?"* → index 1. *"So what is everything to its left?"* → the entire left subtree. *"And to its right?"* → the entire right subtree.

**That is the whole algorithm:**
1. Pre-order's front is the root
2. Find the root in in-order — everything left is the left subtree, everything right is the right subtree
3. Recurse on both halves

```python
def build_tree(preorder, inorder):
    idx = {v: i for i, v in enumerate(inorder)}     # O(1) lookup, not O(n) search
    self_pos = 0
    def build(lo, hi):
        nonlocal self_pos
        if lo > hi: return None
        root_val = preorder[self_pos]; self_pos += 1
        node = TreeNode(root_val)
        mid = idx[root_val]
        node.left  = build(lo, mid - 1)      # must build LEFT first —
        node.right = build(mid + 1, hi)      # pre-order consumes left subtree next
        return node
    return build(0, len(inorder) - 1)
```

**Two things to make explicit:**
- The index map turns an O(n) `list.index()` inside recursion into O(1), taking the whole thing from O(n²) to O(n). A direct application of Week 1.
- **Left must be built before right**, because pre-order emits the entire left subtree before the right. Order of the two lines is load-bearing.

**Part B — why pre + post does not work.** Ask them. With pre-order and post-order you cannot tell where the left subtree ends when a node has only one child — both orderings look identical for a left-only and a right-only child. **In-order is what supplies the split point.** A genuinely good interview follow-up question.

**Part C — serialization.** *"How would you write a tree to a string and read it back?"* The key insight: **you must record the nulls.** Without null markers the structure is ambiguous. Pre-order with `#` for null is the standard approach, and deserialization is the same recursion in reverse.

### Live-code (0:45–1:05)
**LC 105 Construct Binary Tree from Preorder and Inorder**, with the index map and the left-before-right ordering explained. Then sketch **LC 297**'s serialize with null markers.

### Guided practice (1:10–1:50)
1. **LC 617 Merge Two Binary Trees** — Easy. Parallel recursion warm-up.
2. **LC 572 Subtree of Another Tree** — Easy. Composes LC 100 from Week 9.
3. **LC 105 Construct Binary Tree from Preorder and Inorder** — Medium
4. **LC 106 Construct Binary Tree from Inorder and Postorder** — Medium. Post-order read backwards gives root, right, left — a good test of whether the idea transferred.

### Live critique (1:50–2:00)
An *LC 105*. Focus: did they build the index map, or call `inorder.index()` inside the recursion? Same output, O(n) versus O(n²). Exactly the kind of thing an interviewer probes.

### Flex (2:00–2:30)
**LC 297 Serialize and Deserialize Binary Tree** — Hard. In the weekend set; start it here.

### Common misconceptions
- Building the right subtree before the left in LC 105.
- Using `list.index()` inside recursion.
- Omitting null markers when serializing.
- Believing any two traversals determine a tree.

### Assignment 10.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 617 Merge Two Binary Trees | Easy | 20 min |
| LC 572 Subtree of Another Tree | Easy | 25 min |
| LC 105 Construct Binary Tree from Preorder and Inorder | Medium | 40 min |
| LC 106 Construct Binary Tree from Inorder and Postorder | Medium | 35 min |

---

## Session 2 (Wednesday) — Heaps & Priority Queues

### Warm-up (0:00–0:10)
1. Why can't pre-order + post-order reconstruct a tree?
2. Why is an index map needed in LC 105?
3. You need the 5 largest of a million numbers. Sorting is O(n log n). Can we do better?

Question 3 opens the session.

### Concept spine (0:10–0:45)

**Part A — what a heap actually is.**

> A **complete binary tree** where every parent is ≤ (min-heap) or ≥ (max-heap) both children. **Only the root is guaranteed extreme — the rest is only loosely ordered.** That weaker guarantee is exactly why it is cheap to maintain.

- `peek` → **O(1)** (the root)
- `push` → **O(log n)** (add at the end, bubble up)
- `pop` → **O(log n)** (swap root with last, remove, bubble down)
- `heapify` a list → **O(n)**, not O(n log n) — worth a sentence, it surprises people

Stored as an array, not with node objects: children of `i` are `2i+1` and `2i+2`. Draw both views side by side.

**Part B — the top-K argument.** Return to the warm-up question.

> *"Keep a **min**-heap of size k. For each element: push it; if the heap exceeds size k, pop the smallest. At the end the heap holds the k largest, and its root is the k-th largest."*

**Cost: O(n log k).** For n = 1,000,000 and k = 5, log k is about 2 while log n is about 20 — **a tenfold difference**, and O(1) memory in k rather than n.

*"Counter-intuitively, you use a min-heap to find the largest elements. The root is the weakest survivor, so it is the one to evict."* Students find this backwards at first; make them explain it back.

**Part C — Python's `heapq`.** It is a **min-heap only**. For a max-heap, negate:
```python
import heapq
heapq.heappush(h, -x)
largest = -heapq.heappop(h)
heapq.heappush(h, (priority, item))      # tuples compare element-wise
```
Warn about ties: if `priority` ties and `item` is not comparable, the comparison raises. Push `(priority, counter, item)`.

**JavaScript has no built-in heap.** Students solving in JS must implement one — flag it now, not during the contest.

**Part D — the two-heap median (LC 295).** *"Track the median of a growing stream."*

> A **max-heap for the smaller half**, a **min-heap for the larger half**, kept balanced in size. The median is the top of one heap, or the average of both tops.

Draw it. The invariant — `max(lower) ≤ min(upper)` and sizes differ by at most one — is what makes it work, and it is a good example of maintaining a two-part structure.

**Part E — revisit LC 215 and LC 347.** They solved LC 215 with quickselect in Week 7 and LC 347 with counting in Week 1. Now show the heap solutions and compare honestly:

| Problem | Approach | Time | When preferred |
|---|---|---|---|
| LC 215 | Sort | O(n log n) | never, but simplest to state |
| LC 215 | Min-heap size k | O(n log k) | streaming, or k ≪ n |
| LC 215 | Quickselect | O(n) average | offline, one query, no worst-case guarantee needed |
| LC 347 | Heap | O(n log k) | general |
| LC 347 | Bucket sort by frequency | **O(n)** | frequencies bounded by n — the best here |

> *"Three correct solutions with different trade-offs. In an interview, saying that sentence out loud is worth more than any one of them."*

### Live-code (0:45–1:05)
`heapq` basics, then **LC 215 with a size-k heap**, then **LC 295 Find Median from Data Stream** with two heaps.

### Guided practice (1:10–1:50)
1. **LC 1046 Last Stone Weight** — Easy. Max-heap via negation.
2. **LC 703 Kth Largest Element in a Stream** — Easy. The size-k heap, literally.
3. **LC 973 K Closest Points to Origin** — Medium. Heap on a computed key; note you can compare squared distances and skip the square root.
4. **LC 295 Find Median from Data Stream** — Hard

### Live critique (1:50–2:00)
An *LC 973*. Focus: did they sort (O(n log n)) or use a size-k heap (O(n log k))? Both are accepted by LeetCode. Ask which they would say in an interview and why — and whether they computed square roots unnecessarily.

### Flex (2:00–2:30)
**LC 23 Merge k Sorted Lists**, with a heap. They solved it by divide and conquer in Week 7. Both are O(N log k). Comparing the two solutions side by side is the clearest demonstration this term that structure choice is a *decision*, not a lookup.

### Common misconceptions
- Using a max-heap for top-K largest. It is a min-heap of size k.
- Forgetting Python's `heapq` is min-only.
- Believing a heap is fully sorted.
- Tuple comparison raising on ties with non-comparable payloads.
- In LC 295, letting the heap sizes drift out of balance.

### Assignment 10.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1046 Last Stone Weight | Easy | 20 min |
| LC 703 Kth Largest Element in a Stream | Easy | 20 min |
| LC 973 K Closest Points to Origin | Medium | 30 min |
| LC 347 Top K Frequent Elements — **re-solve with a heap**, then with bucket sort | Medium | 30 min |
| LC 23 Merge k Sorted Lists — **re-solve with a heap** | Hard | 30 min |

The two re-solves are the point. Students write a one-line comparison of each pair of approaches in the file header.

---

## Session 3 (Thursday) — Tries

### Warm-up (0:00–0:10)
1. Min-heap or max-heap for the k largest, and why?
2. What is the complexity of `heapify` on a list of n items?
3. What are the two heaps in LC 295, and what invariant links them?

### Concept spine (0:10–0:45)

**Part A — the motivating problem.** *"You have a dictionary of 100,000 words. Given a prefix, return every word starting with it. How?"*

A hash set gives O(1) exact lookup but nothing for prefixes. Scanning all words is O(n·L).

> **A trie: a tree where each edge is one character, and words sharing a prefix share a path.**

Draw a trie for `["cat", "car", "card", "dog"]` on the board. Show `cat` and `car` sharing `c-a`.

```python
class TrieNode:
    def __init__(self):
        self.children = {}        # char -> TrieNode
        self.is_end = False       # a word ENDS here

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end = True

    def _walk(self, prefix):
        node = self.root
        for ch in prefix:
            if ch not in node.children: return None
            node = node.children[ch]
        return node

    def search(self, word):
        node = self._walk(word)
        return node is not None and node.is_end

    def startsWith(self, prefix):
        return self._walk(prefix) is not None
```

**`is_end` is the crux.** Without it you cannot distinguish "car is a stored word" from "car is only a prefix of card". Ask what breaks if you remove it.

**Complexity:** insert and search are **O(L)** in the word length, *independent of how many words are stored*. That is the selling point.

**Part B — wildcards (LC 211).** `.` matches any character. At a `.`, you must try **every** child — so the walk becomes a recursive backtracking search. First combination of a trie with recursion.

**Part C — the inversion (LC 212), which is the week's big idea.**

*"You have a grid and 1,000 words to find in it. Obvious approach: for each word, search the grid. Cost: 1,000 grid searches."*

*"Now invert it: put all 1,000 words into a trie, then walk the grid **once**, carrying a trie pointer alongside. The instant the current path is not a prefix in the trie, prune — every remaining word down that path is impossible."*

> **The general principle, worth naming: instead of testing many candidates one at a time, build a structure that lets you reject whole families of candidates at once.**

That sentence transfers well beyond tries.

### Live-code (0:45–1:10)
**LC 208 Implement Trie** in full, then the recursive wildcard search for **LC 211**.

### Guided practice (1:15–1:50)
1. **LC 208 Implement Trie (Prefix Tree)** — Medium
2. **LC 648 Replace Words** — Medium. Walk each word down the trie, stop at the first `is_end`.
3. **LC 211 Design Add and Search Words Data Structure** — Medium

### Live critique (1:50–2:00)
An *LC 208*. Focus: a `children` dict versus a fixed 26-slot array. Both are fine — the array is faster and larger, the dict is flexible for arbitrary character sets. A good, concrete trade-off conversation.

### Flex (2:00–2:30)
**LC 212 Word Search II** — Hard. Trie plus grid backtracking. It is in the weekend set; start it here. This problem is a genuine payoff for the whole term and is worth showing even partially.

### Common misconceptions
- Omitting `is_end`, so every prefix reports as a word.
- Sharing one `TrieNode` across branches (aliasing again).
- Believing a trie is a BST.
- In LC 212, not pruning — which loses the entire advantage.

### Assignment 10.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 208 Implement Trie (Prefix Tree) | Medium | 35 min |
| LC 648 Replace Words | Medium | 30 min |
| LC 211 Design Add and Search Words Data Structure | Medium | 45 min |

---

## Session 4 (Friday) — ARENA: Contest 6

**Mode:** Contest · 90 minutes

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 257 Binary Tree Paths | Easy | Tree DFS with path tracking |
| P2 | LC 653 Two Sum IV — Input is a BST | Easy | Combines Week 1 hashing with Week 9 trees |
| P3 | LC 692 Top K Frequent Words | Medium | Heap with a custom comparator |
| P4 | LC 480 Sliding Window Median | Hard | Two heaps **inside** a sliding window |

**Calibration:** P2 is the nice one — it rewards students who see that a BST traversal plus a hash set is just Two Sum again. P4 composes Week 3's window with today's two-heap structure and will be solved by at most one or two.

### Reveal (1:45–2:00)
Explain **P3**'s comparator trap: sort by frequency descending *and* lexicographically ascending. In Python this needs `(-count, word)` as the heap key, and getting the sign right is where most failed attempts land.

For **P4**, show the composition explicitly: *"You already know the window and you already know the two heaps. The only new problem is removing an element from the middle of a heap — and 'lazy deletion' is the standard answer."*

### Assignment 10.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 692 Top K Frequent Words** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 10 (6h) — due Tuesday, Week 11

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 116 Populating Next Right Pointers in Each Node | Medium | 35 min |
| LC 1268 Search Suggestions System | Medium | 40 min |
| LC 677 Map Sum Pairs | Medium | 35 min |
| LC 297 Serialize and Deserialize Binary Tree | Hard | 55 min |
| LC 212 Word Search II | Hard | 60 min |

**LC 116** has an O(1)-space solution that uses the `next` pointers already built on the level above — a lovely trick worth finding. **LC 212** is the week's summit.

### Spaced revision (1h) — Weeks 9, 7, 4
| Problem | Source | Target time |
|---|---|---|
| LC 543 Diameter of Binary Tree | Week 9 (N−1) | under 12 min |
| LC 50 Pow(x, n) | Week 7 (N−3) | under 12 min |

### Written editorial (1h)
**LC 212 Word Search II.**

The observation to look for: *"Do not search the grid once per word. Build a trie of all the words, then do a single DFS over the grid carrying a trie pointer — the moment the current path is not a prefix of any word, prune the entire branch. This turns 'test every candidate' into 'reject whole families of candidates at once.'"*

A student who articulates the inversion has understood tries. A student who describes the DFS mechanics without it has copied a shape.

---

## Instructor notes

### What usually goes wrong this week
- **The min-heap-for-max-K inversion.** Expect confusion; make students explain the eviction logic aloud rather than restating it yourself.
- **JavaScript students hit the missing-heap wall.** Warn them Tuesday so they can write one before Wednesday's assignment.
- **Tries feel easy until LC 212**, which combines them with backtracking students have not formally learned yet (Week 15). That is deliberate — meeting backtracking informally here makes Week 15 easier. Say so if it comes up.
- **This is a heavy week: three structures.** If time is tight, tries can lose Thursday's flex; heaps cannot lose anything.

### Watch list
The three re-solves (LC 215, LC 347, LC 23) are a good measure of maturity. A student who can articulate *why* they would choose one approach over another in an interview is ready for Phase III. One who only knows "the way we did it in class" needs the discrimination drills from `01-instructor/04-spaced-revision-system.md`, §5.

### What to cut if you are behind
1. LC 106 from Tuesday — LC 105 carries the idea
2. LC 677 and LC 116 from the weekend set
3. LC 648 from Thursday's assignment
4. Tuesday's flex (LC 297) — but keep it in the weekend set

**Never cut:** the top-K min-heap argument, the two-heap median, the trie `is_end` discussion, or the LC 212 inversion — even if you only describe it.
