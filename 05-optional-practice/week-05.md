# Week 5 — Linked Lists · Optional Practice

**Phase I** · Week band: **Medium** · Reinforce = Medium (see below) · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> **A note on this week's Reinforce track.** Almost every *Easy* linked-list problem on LeetCode is already in the standard set — Weeks 5's assignments take LC 21, 83, 141, 203, 206, 234, 876 and 1290 between them. So the Reinforce track here is gentle **Medium** rather than Easy. If those are still too much, the right move is not a different problem: it is re-solving Tuesday's standard set from a blank file until the pointer moves stop feeling arbitrary.

---

## Session 1 (Tuesday) — Classes, Nodes & Traversal

> Optional. Not graded. Skip freely.

### Check your understanding
1. Draw a three-node list on paper. Now write the four lines that insert a node between the first and second. Which line must come first, and what breaks if you swap them?
2. What is the difference between `node.next = x` and `node = x` inside a traversal loop? Which one actually changes the list?
3. Why does almost every linked-list solution start by creating a dummy head node?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 705 Design HashSet | Easy | 20 min |
| LC 1603 Design Parking System | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 237 Delete Node in a Linked List | Medium | 40 min |
| LC 460 LFU Cache | **Hard** | 55 min |

LC 460 is the harder sibling of the LRU cache in Week 15's design session. Reading it now makes that session much easier — the insight is that O(1) eviction needs a linked list *and* a hash map pointing into it.

### 3. Revision — Week 4 (Strings & Matrices) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 168 Excel Sheet Column Title | Easy | 25 min |
| LC 43 Multiply Strings | Medium | 35 min |

### 4. Interview follow-ups
1. **"Why a dummy head?"** — A good answer says it removes the special case where the node being removed or inserted is the head, so one code path handles every position. That is a real design argument, not a trick.
2. **"How would you test this without a debugger?"** — A good answer walks a 0-, 1- and 2-node list by hand. Linked-list bugs live almost entirely in those three cases, and saying so unprompted signals real experience.

---

## Session 2 (Wednesday) — Reversal, Cycles & Merging

> Optional. Not graded. Skip freely.

### Check your understanding
1. Reversing a list needs three pointers. Name them and say what each one holds at the top of the loop.
2. Floyd's cycle detection: why is the meeting point *not* the start of the cycle, and what do you do next to find it?
3. Why does the fast pointer move two steps rather than three?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 82 Remove Duplicates from Sorted List II | Medium | 35 min |
| LC 2095 Delete the Middle Node of a Linked List | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 147 Insertion Sort List | Medium | 40 min |
| LC 2296 Design a Text Editor | **Hard** | 55 min |

LC 147 is a sorting algorithm implemented on pointers — slow by design, and the point is the pointer discipline, not the complexity.

### 3. Revision — Week 4 (Strings & Matrices) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 709 To Lower Case | Easy | 25 min |
| LC 299 Bulls and Cows | Medium | 35 min |

### 4. Interview follow-ups
1. **"Prove the fast and slow pointers must meet if there is a cycle."** — A good answer argues that once both are inside the cycle the gap closes by exactly one each step, so it must reach zero. This is the most-asked follow-up in the whole linked-list topic.
2. **"What is the space complexity, and how does it compare to using a set of visited nodes?"** — A good answer gives O(1) versus O(n) and notes the set version is easier to write and often perfectly acceptable — then says which one it would offer first and why.

---

## Session 3 (Thursday) — Pointer Surgery

> Optional. Not graded. Skip freely.

### Check your understanding
1. "Pointer surgery" problems are mostly about ordering the reassignments. What is the general rule for the order?
2. You need the node *before* the one you want to remove. Two ways to get it — name both.
3. When is it correct to modify node values instead of relinking nodes, and when is that cheating?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 725 Split Linked List in Parts | Medium | 35 min |
| LC 2181 Merge Nodes in Between Zeros | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 3217 Delete Nodes From Linked List Present in Array | Medium | 40 min |
| LC 1206 Design Skiplist | **Hard** | 55 min |

### 3. Revision — Week 4 (Strings & Matrices) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 657 Robot Return to Origin | Easy | 25 min |
| LC 937 Reorder Data in Log Files | Medium | 35 min |

### 4. Interview follow-ups
1. **"You changed the node's value instead of relinking. Is that acceptable?"** — A good answer says it depends on whether anything else holds references to those nodes. If it does, mutating values is a visible side effect and relinking is correct. Asking is better than assuming.
2. **"What if it were a doubly linked list?"** — A good answer notes that every operation now maintains two pointers instead of one, that removal becomes O(1) given the node, and that forgetting the `prev` update is the classic bug.

---

## Session 4 (Friday) — ARENA: Contest 3

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. From memory, in a blank file: reverse a linked list, iteratively and recursively. Which did you get right first?
2. Which contest problem cost you the most time, and was it the idea or the pointer bookkeeping?
3. Name the three problems from this week you would want to re-solve before Week 9 (trees reuse all of this).

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2058 Find the Minimum and Maximum Number of Nodes Between Critical Points | Medium | 35 min |
| LC 2807 Insert Greatest Common Divisors in Linked List | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 382 Linked List Random Node | Medium | 40 min |
| LC 381 Insert Delete GetRandom O(1) - Duplicates allowed | **Hard** | 55 min |

LC 381 is a design Hard built on the containers from Week 1 plus this week's structural thinking. It is a legitimate preview of Week 15 and a good contest-day stretch.

### 3. Revision — Week 4 (Strings & Matrices) · Week 2 (Arrays I)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2011 Final Value of Variable After Performing Operations | Easy | 25 min |
| LC 1689 Partitioning Into Minimum Number Of Deci-Binary Numbers | Medium | 35 min |

### 4. Interview follow-ups
1. **"This week's structures reappear in Week 9. What carries over?"** — A good answer names it precisely: a tree node is a node with two `next` pointers, and recursive traversal is the same shape as recursive list traversal. Seeing that now makes Week 9 much cheaper.
2. **"Your solution is recursive. What is the space complexity?"** — A good answer counts the call stack — O(n) for a list of n nodes — and notes that the iterative version is O(1). Candidates routinely claim O(1) for recursive solutions; do not be one of them.
