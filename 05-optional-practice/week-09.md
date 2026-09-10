# Week 9 — Trees I · Optional Practice

**Phase II** · Week band: **Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Trees are where Week 7 pays off. If recursion landed, this week feels almost easy and the Reinforce track is where the volume is worth having — tree problems reward repetition more than any other topic, because the shape is always the same and only the work at each node changes.
>
> Friday is **Mock Round 2** — the real one. The Interview track this week is preparation, not decoration.

---

## Session 1 (Tuesday) — Trees & DFS Traversal

> Optional. Not graded. Skip freely.

### Check your understanding
1. Preorder, inorder, postorder — the recursive code differs by one line's position. Which line, and what does each order guarantee about when a node is visited relative to its children?
2. Which traversal of a BST comes out sorted, and why?
3. What is the space complexity of a recursive tree traversal on a balanced tree, and on a degenerate one?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 144 Binary Tree Preorder Traversal | Easy | 20 min |
| LC 145 Binary Tree Postorder Traversal | Easy | 25 min |

LC 144 and LC 145 look trivial recursively — do them **iteratively**. The iterative postorder in particular is where the call stack stops being abstract, and it is a real interview question.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 103 Binary Tree Zigzag Level Order Traversal | Medium | 40 min |
| LC 987 Vertical Order Traversal of a Binary Tree | **Hard** | 55 min |

LC 987 needs a traversal plus a sort by two keys, and it punishes any confusion about what your recursion carries down versus returns up.

### 3. Revision — Week 8 (Binary Search & Sorting) · Week 6 (Stacks & Queues) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 589 N-ary Tree Preorder Traversal | Easy | 25 min |
| LC 114 Flatten Binary Tree to Linked List | Medium | 35 min |

### 4. Interview follow-ups
1. **"Can you do that traversal without recursion?"** — A good answer describes the explicit stack and what each frame holds. Week 6's stack session and Week 7's call-stack model are exactly the preparation for this, and interviewers ask it to check whether you understand recursion or just use it.
2. **"What is the space complexity, honestly?"** — A good answer says O(h) where h is the height, then gives the two bounds: O(log n) balanced, O(n) degenerate. Saying "O(1), I'm not allocating anything" is the mistake — the call stack is space.

---

## Session 2 (Wednesday) — BFS, Depth & Path Problems

> Optional. Not graded. Skip freely.

### Check your understanding
1. BFS on a tree needs a queue. What do you do at the start of each iteration to process exactly one level at a time?
2. Diameter of a binary tree: what does your recursive function *return*, and what does it *record* on the side? Why are those different?
3. For a root-to-leaf path problem, what do you have to remember to do after the recursive calls return?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 111 Minimum Depth of Binary Tree | Easy | 20 min |
| LC 938 Range Sum of BST | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 701 Insert into a Binary Search Tree | Medium | 40 min |
| LC 815 Bus Routes | **Hard** | 55 min |

LC 815 is BFS on a graph you have to construct first, and it is a legitimate preview of Week 11. The hard part is deciding what a "node" is.

### 3. Revision — Week 8 (Binary Search & Sorting) · Week 6 (Stacks & Queues) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 976 Largest Perimeter Triangle | Easy | 25 min |
| LC 1008 Construct Binary Search Tree from Preorder Traversal | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why does the diameter function return the height rather than the diameter?"** — A good answer separates the two: the answer might not pass through the current root, so the diameter is recorded globally while the recursion returns the one thing the parent needs. This return-versus-record distinction is the core tree insight.
2. **"BFS or DFS for this problem?"** — A good answer picks on the basis of what is being asked — shortest/level-based favours BFS, path/subtree properties favour DFS — rather than on preference, and says so.

---

## Session 3 (Thursday) — Binary Search Trees

> Optional. Not graded. Skip freely.

### Check your understanding
1. State the BST invariant precisely. Why is "left child < node < right child" not sufficient?
2. Validating a BST needs bounds passed down the recursion. What are the initial bounds, and how do they narrow?
3. Deleting a node with two children: which node replaces it, and why are there two valid answers?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 530 Minimum Absolute Difference in BST | Easy | 20 min |
| LC 501 Find Mode in Binary Search Tree | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 450 Delete Node in a BST | Medium | 40 min |
| LC 1569 Number of Ways to Reorder Array to Get Same BST | **Hard** | 55 min |

LC 1569 is a combinatorics problem wearing a BST costume — well out of band, and included because the counting argument is genuinely beautiful. Read the editorial without guilt.

### 3. Revision — Week 8 (Binary Search & Sorting) · Week 6 (Stacks & Queues) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 590 N-ary Tree Postorder Traversal | Easy | 25 min |
| LC 341 Flatten Nested List Iterator | Medium | 35 min |

### 4. Interview follow-ups
1. **"Your validation compares each node to its parent. Is that correct?"** — A good answer says no and gives the counterexample: a node deep in the left subtree can exceed the root while still satisfying every parent comparison. Bounds must be inherited, not local.
2. **"The tree is unbalanced. What happens to your complexity?"** — A good answer says search degrades from O(log n) to O(n), notes that a degenerate BST is a linked list, and mentions that balancing exists without claiming to implement rotations — which this course deliberately does not cover.

---

## Session 4 (Friday) — ARENA: Mock Interview Round 2

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Mock day, round two. Last time you were told what your failure pattern was. What was it, and what have you changed?
2. From memory, in a blank file: all three DFS traversals iteratively, and level-order BFS. Twenty minutes.
3. Given an unfamiliar tree problem, what are the two questions you ask yourself before writing anything?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 463 Island Perimeter | Easy | 20 min |
| LC 404 Sum of Left Leaves | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1448 Count Good Nodes in Binary Tree | Medium | 40 min |
| LC 1028 Recover a Tree From Preorder Traversal | **Hard** | 55 min |

### 3. Revision — Week 8 (Binary Search & Sorting) · Week 6 (Stacks & Queues) · Week 3 (Two Pointers & Window)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1122 Relative Sort Array | Easy | 25 min |
| LC 633 Sum of Square Numbers | Medium | 35 min |

### 4. Interview follow-ups
1. **"Talk me through your approach before you code."** — A good answer states the traversal choice, what the recursion returns, what it records, and the base case — in about four sentences. Doing this unprompted is the biggest single scoring difference in a tree interview.
2. **"What if the tree were very deep — say a million nodes in a line?"** — A good answer identifies stack overflow as a real risk, gives the iterative alternative, and states the depth at which the language's recursion limit bites. This is a systems answer, and it separates strong candidates.
