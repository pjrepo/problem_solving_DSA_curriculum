# Week 9 — Trees I: Traversal, BFS & Binary Search Trees

**Phase II: Techniques** · Difficulty band: **Medium** · Friday: **Mock Interview Round 2**

> Trees are where Week 7's recursion pays off. A tree *is* a recursive structure — a node with two subtrees, each of which is a tree — so most tree solutions are three or four lines once students trust the recursive call. Students who genuinely internalised the leap of faith will find this week satisfying; students who did not will struggle badly, and that is your clearest possible signal.

---

## Exit criteria

- [ ] Define a `TreeNode` and explain why a tree is a recursive structure
- [ ] Write pre-order, in-order and post-order traversal, and say what distinguishes them
- [ ] Explain the difference between "processing on the way down" and "on the way up"
- [ ] Write BFS on a tree with correct level separation
- [ ] Use the "return one value, track another" idiom (diameter, max path sum)
- [ ] State the BST invariant and explain why in-order traversal yields sorted output
- [ ] Validate a BST by passing down a range, and say why comparing with the parent fails

---

## Session 1 (Tuesday) — Trees & DFS Traversal

### Weekend-set debrief (0:00–0:15)
Debrief the **LC 74 vs LC 240** pairing — students who binary-searched LC 240 got it wrong, which is exactly the lesson about checking whether a structure's guarantee actually holds. Then **LC 410**: check whether the editorials argued monotonicity or merely asserted it.

### Concept spine (0:15–0:45)

**Part A — the structure.**

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

Draw a tree. Then circle a subtree and say the sentence that unlocks the week:

> *"This subtree is itself a tree. That is the whole idea. A tree is a node plus two smaller trees — so almost every tree function is: handle the empty case, ask the same function about the left and right subtrees, and combine."*

**The three questions from Week 7, specialised for trees:**
1. **Base case:** `if not node: return ...` — nearly always the first line
2. **Smaller subproblem:** `left = f(node.left)`, `right = f(node.right)`
3. **Combine:** what do I do with `left`, `right` and `node.val`?

**Show how short this makes things:**

```python
def max_depth(root):
    if not root: return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

*"Three lines. You could not write that in Week 6."* Make the progress explicit — it is genuinely motivating at this point in the term.

**Part B — the three traversals, and what actually differs.**

```python
def preorder(node):    def inorder(node):     def postorder(node):
    if not node: return    if not node: return    if not node: return
    visit(node)            inorder(node.left)     postorder(node.left)
    preorder(node.left)    visit(node)            postorder(node.right)
    preorder(node.right)   inorder(node.right)    visit(node)
```

> **The only difference is *when* you visit the node relative to the recursive calls.** Everything else is identical.

When each is the right choice:
- **Pre-order** — process the node before its children (copying a tree, serialising)
- **In-order** — **on a BST, this yields sorted order.** The single most useful fact about BSTs.
- **Post-order** — need the children's answers first (height, deletion, most "compute a value" problems)

*"Most problems that compute something about a subtree are post-order, because you need the children's results before you can produce yours."*

**Part C — down versus up.** This distinction is worth a full five minutes because it recurs through Weeks 9–15.

| Direction | Mechanism | Example |
|---|---|---|
| Information flows **down** | passed as parameters | current depth, valid range for BST validation, path sum so far |
| Information flows **up** | via return values | height, subtree sums, "does this subtree contain X" |

*"When you get stuck on a tree problem, ask: does this node need to know something from its ancestors (pass it down), or from its descendants (return it up)? Sometimes both — that is what makes diameter hard."*

### Live-code (0:45–1:05)
`max_depth`, then **LC 226 Invert Binary Tree** (three lines), then in-order traversal **both recursively and with an explicit stack**. The iterative version connects Week 6's stack to Week 7's recursion: *"You are doing by hand exactly what the language was doing for you."*

### Guided practice (1:10–1:50)
1. **LC 104 Maximum Depth of Binary Tree** — Easy
2. **LC 226 Invert Binary Tree** — Easy
3. **LC 100 Same Tree** — Easy. Two trees recursed in parallel.
4. **LC 101 Symmetric Tree** — Easy. The helper takes *two* nodes — a genuine step up in recursive thinking.

LC 101 is the one to watch. Students who try to write it with a single-node helper will get stuck, and realising you can choose your own helper signature is an important unlock.

### Live critique (1:50–2:00)
An *LC 101*. Focus: did they define a two-argument helper, or attempt it with the given signature? Discuss designing a helper whose signature suits the recursion — a genuinely transferable habit.

### Flex (2:00–2:30)
**LC 94 Binary Tree Inorder Traversal**, iteratively with an explicit stack. Harder than it looks and directly reinforces the recursion/stack equivalence.

### Common misconceptions
- Missing the `if not node` base case → `AttributeError` on `None`.
- Confusing height (edges/nodes downward) with depth (distance from the root). Define both.
- Believing a "binary tree" is sorted — that is a *binary search tree*.
- Trying to trace the whole recursion mentally instead of trusting one level.

### Assignment 9.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 104 Maximum Depth of Binary Tree | Easy | 15 min |
| LC 226 Invert Binary Tree | Easy | 15 min |
| LC 100 Same Tree | Easy | 20 min |
| LC 101 Symmetric Tree | Easy | 30 min |
| LC 94 Binary Tree Inorder Traversal (recursive **and** iterative) | Easy | 40 min |

---

## Session 2 (Wednesday) — BFS, Depth & Path Problems

### Warm-up (0:00–0:10)
1. What is the only difference between pre-, in- and post-order?
2. Write `max_depth` from memory.
3. Which traversal gives sorted output on a BST?

### Concept spine (0:10–0:45)

**Part A — BFS on a tree.** *"How would you print a tree level by level?"* Recursion goes depth-first naturally; this needs breadth.

> **A queue. That is the entire algorithm.**

```python
from collections import deque

def level_order(root):
    if not root: return []
    result, q = [], deque([root])
    while q:
        level_size = len(q)               # THE key line — freeze this level's size
        level = []
        for _ in range(level_size):       # process exactly this level
            node = q.popleft()
            level.append(node.val)
            if node.left:  q.append(node.left)
            if node.right: q.append(node.right)
        result.append(level)
    return result
```

**`level_size = len(q)` is the line that matters.** Without capturing the size before the inner loop, you cannot tell where one level ends and the next begins. Have students explain why.

**Then plant Week 11 explicitly:**

> *"Remember this. In two weeks we do BFS on graphs, and it is this code with `node.left/right` replaced by `for neighbour in graph[node]` and a visited set added because graphs can have cycles. Trees are graphs without cycles. You are learning graph BFS right now."*

**Part B — the "return one thing, track another" idiom.** The hardest idea in tree problems, and it recurs in Weeks 12 and 14.

**LC 543 Diameter.** The longest path between any two nodes. It may or may not pass through the root.

The tension: to compute the diameter *through* a node you need both subtree heights — but your parent needs your **height**, not your diameter. You cannot return both in one value.

**The resolution:** return the height, and record the best diameter in an outer variable as a side effect.

```python
def diameter_of_binary_tree(root):
    best = 0
    def height(node):
        nonlocal best
        if not node: return 0
        L, R = height(node.left), height(node.right)
        best = max(best, L + R)        # the answer THROUGH this node
        return 1 + max(L, R)           # what the PARENT needs
    height(root)
    return best
```

**Say it as a rule:** *"When the answer at a node and the value your parent needs are different things, return what the parent needs and record the answer on the side."* Once heard, LC 124, LC 687 and LC 250 all become the same problem.

### Live-code (0:45–1:05)
**LC 102 Level Order Traversal**, then **LC 543 Diameter** with the idiom made explicit.

### Guided practice (1:10–1:50)
1. **LC 102 Binary Tree Level Order Traversal** — Medium
2. **LC 199 Binary Tree Right Side View** — Medium. BFS taking the last of each level (or DFS tracking depth — both worth discussing).
3. **LC 110 Balanced Binary Tree** — Easy. The naive version is O(n²); the post-order version with an early exit is O(n). A good complexity conversation.
4. **LC 543 Diameter of Binary Tree** — Easy (rated), genuinely Medium in difficulty

### Live critique (1:50–2:00)
An *LC 110*. Focus: is it O(n) or O(n²)? Calling `height()` at every node re-walks the tree. This is a case where a correct solution has the wrong complexity, and the author usually has not noticed.

### Flex (2:00–2:30)
**LC 124 Binary Tree Maximum Path Sum** — Hard. The same idiom with one twist: a negative subtree contributes nothing, so clamp at zero. Worth the full block.

### Common misconceptions
- Forgetting `level_size` and flattening all levels together.
- Using a list with `pop(0)` instead of a `deque` — O(n) per operation.
- In LC 543, returning the diameter instead of the height.
- In LC 124, forgetting to clamp negative contributions to zero.

### Assignment 9.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 102 Binary Tree Level Order Traversal | Medium | 30 min |
| LC 199 Binary Tree Right Side View | Medium | 30 min |
| LC 110 Balanced Binary Tree | Easy | 25 min |
| LC 543 Diameter of Binary Tree | Easy | 30 min |
| LC 112 Path Sum | Easy | 20 min |

---

## Session 3 (Thursday) — Binary Search Trees

### Warm-up (0:00–0:10)
1. Why does `level_size = len(q)` matter?
2. State the "return one thing, track another" rule.
3. What is a BST's defining property?

### Concept spine (0:10–0:45)

**Part A — the invariant, stated precisely.**

> For **every** node: all values in the left subtree are less than the node, and all values in the right subtree are greater.

**Emphasise "every" and "all subtree" — not just the immediate children.** This is exactly where LC 98 traps people.

**Two consequences that do all the work:**
1. **Search prunes half the tree** — O(h), which is O(log n) if balanced
2. **In-order traversal yields sorted output** — the most exploitable fact about BSTs

**Part B — LC 98 Validate BST**, and the trap.

Write the wrong solution on the board first:

```python
def is_valid(node):                     # WRONG
    if not node: return True
    if node.left and node.left.val >= node.val: return False
    if node.right and node.right.val <= node.val: return False
    return is_valid(node.left) and is_valid(node.right)
```

Then the counterexample:
```
      5
     / \
    1   7
       / \
      3   8      <- 3 < 5, but it is in the RIGHT subtree of 5
```
Every parent-child pair is locally fine; the tree is not a BST. **Let the room find why before you say it.**

**The fix — pass a valid range down.** (This is Tuesday's "information flows down" in action.)

```python
def is_valid_bst(root):
    def check(node, lo, hi):
        if not node: return True
        if not (lo < node.val < hi): return False
        return check(node.left, lo, node.val) and check(node.right, node.val, hi)
    return check(root, float('-inf'), float('inf'))
```

**The alternative:** in-order traverse and verify the sequence is strictly increasing. Show both — the second is often what students find first, and it is perfectly good.

**Part C — exploiting in-order.** **LC 230 Kth Smallest**: in-order traverse and stop at the kth. With an explicit stack you can stop early rather than walking the whole tree — a good follow-up an interviewer will ask for.

**Part D — LCA in a BST (LC 235).** Walk down from the root: if both targets are smaller go left, if both larger go right; **the moment they split, you are at the LCA.** O(h), no recursion into both sides. Then contrast with LC 236 (general binary tree), where you must search both subtrees because there is no ordering to exploit. **That contrast is the lesson: a BST's ordering is what buys the shortcut.**

### Live-code (0:45–1:05)
**LC 98** with range passing (after showing the broken version), then **LC 230** with an iterative in-order and early exit.

### Guided practice (1:10–1:50)
1. **LC 700 Search in a Binary Search Tree** — Easy
2. **LC 98 Validate Binary Search Tree** — Medium
3. **LC 230 Kth Smallest Element in a BST** — Medium
4. **LC 235 Lowest Common Ancestor of a BST** — Medium

### Live critique (1:50–2:00)
An *LC 98*. Focus: range-passing or in-order? If in-order, did they materialise the whole list (O(n) space) or track only the previous value (O(h))? A clean space-optimisation conversation.

### Flex (2:00–2:30)
**LC 236 Lowest Common Ancestor of a Binary Tree** — Medium. The general case. Elegant, and a genuinely common interview question.

### Common misconceptions
- Validating only against immediate children.
- Using `<=` where the problem requires strict inequality (ask about duplicates — a good clarifying question).
- Believing a BST is automatically balanced. It is not; worst case is a linked list, O(n).
- In LC 235, recursing into both subtrees and losing the O(h) advantage.

### Assignment 9.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 700 Search in a Binary Search Tree | Easy | 15 min |
| LC 98 Validate Binary Search Tree | Medium | 30 min |
| LC 230 Kth Smallest Element in a BST | Medium | 30 min |
| LC 235 Lowest Common Ancestor of a BST | Medium | 25 min |
| LC 108 Convert Sorted Array to Binary Search Tree | Easy | 20 min |

---

## Session 4 (Friday) — ARENA: Mock Interview Round 2

**Mode:** Peer mock interviews · **passing bar: 20/30** (up from 15 in Week 4)

### Framing (0:00–0:10)
Remind them of Round 1's findings — coding too early, silence, no clarifying questions, no testing. **Read out the specific behaviours you recorded in Week 4** (not the names). Then: *"The problems today are Mediums, not Easies. The bar is 20 out of 30. Round 1 was calibration; this one counts."*

### Format
7 pairs · two 45-minute rounds · **re-pair differently from Week 4**. Rotate and score.

### Problem cards (Medium, from Weeks 1–8)
| # | Problem | Follow-up |
|---|---|---|
| 1 | LC 3 Longest Substring Without Repeating Characters | "What if the alphabet is only lowercase letters — can you use O(1) space?" |
| 2 | LC 56 Merge Intervals | "What if intervals arrive as a stream?" |
| 3 | LC 206 Reverse Linked List | "Now do it recursively. What is the space complexity?" |
| 4 | LC 739 Daily Temperatures | "Can you prove it is O(n)?" |
| 5 | LC 875 Koko Eating Bananas | "Why is the feasibility check monotonic?" |
| 6 | LC 53 Maximum Subarray | "Now return the actual subarray, not just the sum." |
| 7 | LC 215 Kth Largest Element in an Array | "Give me two approaches and compare them." |

Every follow-up targets **justification**, not more code. That is where interviews are actually won.

### Group debrief (1:50–2:00)
Compare against Round 1. Usually clarifying questions and narration improve markedly, while **testing and complexity justification stay weak** — those are harder habits. Name that pattern and set the Week 15 target: **23/30, a real phone-screen pass.**

### Assignment 9.4 (2h)
| Task | Budget |
|---|---|
| Re-solve your problem, recorded, out loud. Compare against your Week 4 recording. | 45 min |
| **Written:** your Round 1 reflection said you'd change three things. Did you? Evidence for each. | 25 min |
| Solve **LC 113 Path Sum II** — Medium | 50 min |

Making them audit their own Week 4 commitments is what turns a reflection exercise into an actual behaviour change.

---

## Weekend Set 9 (6h) — due Tuesday, Week 10

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 437 Path Sum III | Medium | 45 min |
| LC 129 Sum Root to Leaf Numbers | Medium | 30 min |
| LC 173 Binary Search Tree Iterator | Medium | 40 min |
| LC 662 Maximum Width of Binary Tree | Medium | 40 min |
| LC 236 Lowest Common Ancestor of a Binary Tree | Medium | 35 min |
| LC 124 Binary Tree Maximum Path Sum | Hard | 50 min |

**LC 437** has a naive O(n²) solution and an O(n) one using a prefix-sum hash map on the current root-to-node path — **it is LC 560 from Week 2 running on a tree.** Point this out only after they have attempted it; the recognition is worth much more if they find it. **LC 173** is the iterative in-order traversal from Tuesday's flex, packaged as a class.

### Spaced revision (1h) — Weeks 8, 6, 3
| Problem | Source | Target time |
|---|---|---|
| LC 56 Merge Intervals | Week 8 (N−1) | under 15 min |
| LC 424 Longest Repeating Character Replacement | Week 3 (N−6) | under 20 min |

### Written editorial (1h)
**LC 124 Binary Tree Maximum Path Sum.**

The observation to look for: *"A node returns to its parent the best **single downward branch** it can offer, but the best path **through** that node combines both branches — so those are two different quantities. Return the first, record the second on the side. And a branch with a negative sum should contribute zero, since you can always decline to extend into it."*

Both halves matter. A student who gets the two-quantities idea but misses the clamping has understood the structure; one who describes the code without either has not.

---

## Instructor notes

### What usually goes wrong this week
- **Students who did not internalise recursion in Week 7 hit a wall here immediately.** This is your unambiguous signal — trees make the gap impossible to hide. Intervene now; Weeks 13–15 will be worse.
- **The "return one thing, track another" idiom needs two exposures.** LC 543 on Wednesday, LC 124 in the weekend. Do not expect it after one.
- **LC 98's local-comparison trap catches nearly everyone.** Let it. The counterexample is the lesson.
- **Students use lists as queues in BFS.** Catch it in critique — it silently degrades complexity.

### Watch list
This is the sharpest diagnostic week in Phase II. A student comfortable with `max_depth` in three lines has the recursive model. A student writing loops and manual stacks for it does not, and will not survive DP. **Cross-reference with your Week 7 paper-tracing diagrams.**

### What to cut if you are behind
1. Tuesday's flex (iterative in-order) — but LC 173 in the weekend needs it, so swap that problem out too
2. LC 108 from Thursday's assignment
3. LC 662 and LC 129 from the weekend set
4. LC 112 from Wednesday's assignment

**Never cut:** the "a tree is a recursive structure" framing, `level_size` in BFS, the "return one thing, track another" idiom, or LC 98's broken-solution demonstration.
