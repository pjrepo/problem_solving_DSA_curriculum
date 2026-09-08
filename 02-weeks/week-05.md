# Week 5 — Linked Lists (and Classes, taught inline)

**Phase I: Foundations** · Difficulty band: **Medium**

> Two things happen this week. Students meet **classes** for the first time — taught at the exact moment a `Node` requires them, not as abstract theory — and they meet a data structure where **references, not indices**, are the unit of thought. That shift is the real difficulty. Everything after this (trees, tries, graphs, LRU cache) depends on being comfortable with "this variable points at that object."

---

## Exit criteria

- [ ] Define a class with `__init__`, attributes and methods, and explain what `self` is
- [ ] Explain the difference between a variable holding an object and a copy of it
- [ ] Draw the pointer state of a linked list operation before writing any code
- [ ] Reverse a linked list iteratively, from memory, with correct pointer ordering
- [ ] Use a **dummy head node** to eliminate edge cases, and explain what it saves
- [ ] Apply fast & slow pointers for cycle detection and midpoint finding
- [ ] State why linked lists give O(1) insertion but O(n) access

---

## Session 1 (Tuesday) — Classes, Nodes & Traversal

### Weekend-set debrief (0:00–0:15)
Debrief **LC 179 Largest Number** — the custom comparator (`a+b > b+a`) surprises people; it previews Week 8. Then **LC 395**, where the standard window template fails. Ask what they tried before giving up; recognising that a familiar tool does not fit is a real skill and worth praising.

### Concept spine (0:15–0:50) — this block is longer than usual; the OOP content needs it

**Part A — why classes, right now.**

Do not open with "object-oriented programming." Open with a problem:

> *"I want to store a sequence where inserting at the front is instant. A list can't do that — `insert(0, x)` is O(n) because everything shifts. What if instead of one block of memory, each element knew where the next one was?"*

Draw it on the board: boxes with a value and an arrow.

*"To build that, I need a thing that holds two pieces of data together — a value and an arrow. That's what a class is for."*

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

**Teach exactly these five things and no more:**
1. `class Name:` defines a new kind of thing
2. `__init__` runs when you create one: `node = ListNode(5)`
3. `self` is *this particular object* — the thing being built or acted on
4. `self.val = val` stores data **on** the object
5. Methods are functions that take `self` and can read and change the object's data

**Part B — the reference model.** This is the part that actually matters, and the part that will bite them.

```python
a = ListNode(1)
b = a                # b is NOT a copy — it is the same object
b.val = 99
print(a.val)         # 99
```

Run it live. Draw two arrows pointing at one box.

**Then the linked-list version of the same idea:**
```python
node.next = other        # "make this node point at other"
node = node.next         # "move my cursor along" — the LIST is unchanged
```

> **The distinction to hammer:** assigning to `node` moves *your cursor*. Assigning to `node.next` changes *the list*. Every linked-list bug students write is a confusion between these two.

**Part C — traversal and the cost model.**

```python
cur = head
while cur:
    print(cur.val)
    cur = cur.next
```

| Operation | Array | Linked list |
|---|---|---|
| Access index i | **O(1)** | O(n) |
| Insert at front | O(n) | **O(1)** |
| Insert after a known node | O(n) | **O(1)** |
| Search by value | O(n) | O(n) |
| Memory | contiguous | scattered + pointer overhead |

*"Linked lists win when you're inserting and deleting a lot and rarely indexing. That is exactly the LRU cache in Week 15."*

**Part D — the dummy head.** Introduce it now and use it all week.

```python
dummy = ListNode(0, head)
prev = dummy
# ... work ...
return dummy.next      # correct even if the original head was removed
```

*"Deleting the head is different from deleting any other node — unless there is a node before the head. The dummy makes every case the same case."* Students who adopt this write dramatically fewer bugs.

### Live-code (0:50–1:05)
Build `ListNode`, a `build(values)` helper and a `to_list(head)` helper — students need these to test locally all week. Then **LC 203 Remove Linked List Elements** using a dummy head.

**JS delta:** `class ListNode { constructor(val=0, next=null) { this.val = val; this.next = next; } }` — same reference semantics.

### Guided practice (1:10–1:50)
1. **LC 876 Middle of the Linked List** — Easy. Two passes first; fast/slow is Wednesday.
2. **LC 203 Remove Linked List Elements** — Easy. Dummy head.
3. **LC 83 Remove Duplicates from Sorted List** — Easy.
4. **LC 1290 Convert Binary Number in a Linked List to Integer** — Easy.

**Insist on drawing before coding.** Give everyone paper. A student who codes a pointer manipulation without drawing it is guessing.

### Live critique (1:50–2:00)
An *LC 203*. Focus: did they use a dummy head or special-case the head with a `while head and head.val == val` prelude? Compare both on the board — the dummy version is visibly shorter and has fewer branches.

### Flex (2:00–2:30)
**LC 707 Design Linked List** — Medium. Implement get, addAtHead, addAtTail, addAtIndex, deleteAtIndex. Tedious but it forces genuine fluency, and it is the best possible use of a flex block this week.

### Common misconceptions
- `node = node.next` versus `node.next = ...` — the central confusion.
- Losing the rest of the list by reassigning `next` before saving it.
- Forgetting `head` may be `None`.
- Thinking a linked list can be indexed.
- Comparing nodes with `==` when identity (`is`) is meant.

### Assignment 5.1 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 876 Middle of the Linked List | Easy | 20 min |
| LC 203 Remove Linked List Elements | Easy | 25 min |
| LC 83 Remove Duplicates from Sorted List | Easy | 25 min |
| LC 1290 Convert Binary Number to Integer | Easy | 20 min |
| LC 707 Design Linked List | Medium | 30 min |

---

## Session 2 (Wednesday) — Reversal, Cycles & Merging

> Reversal is the single most important linked-list skill. It appears inside half of all harder list problems.

### Warm-up (0:00–0:10)
1. What is the difference between `node = node.next` and `node.next = x`?
2. What does a dummy head node save you?
3. `a = ListNode(1); b = a; b.val = 5` — what is `a.val`?

### Concept spine (0:10–0:45)

**Part A — reversal, derived on paper.**

Draw `1 → 2 → 3 → None`. Ask: *"To reverse, node 2 must point at node 1. What do we need to remember before we overwrite `2.next`?"* → node 3, or we lose the rest of the list.

Three pointers, and the order of the four lines is everything:

```python
def reverse_list(head):
    prev = None
    cur = head
    while cur:
        nxt = cur.next      # 1. SAVE the rest, or it's gone forever
        cur.next = prev     # 2. FLIP this node's arrow backwards
        prev = cur          # 3. advance prev
        cur = nxt           # 4. advance cur
    return prev             # cur is None; prev is the new head
```

**Have every student write these four lines on paper, in order, and then trace `1→2→3` line by line.** This is the template to have in muscle memory by Week 16.

**Part B — Floyd's cycle detection.**

*"How do you know a linked list has a cycle, using O(1) space?"* Let them propose a `visited` set — correct, but O(n) space. Push for better.

> Two runners on a circular track at different speeds **must** eventually meet. If the track has an end, the fast one reaches it first.

```python
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow is fast:
        return True
return False
```

**The guard `while fast and fast.next` is the whole correctness argument** — `fast.next.next` requires two nodes ahead. Ask students to explain why both checks are needed.

For **LC 142** (find where the cycle starts): after they meet, reset one pointer to the head and advance both one step at a time; they meet at the cycle entrance. **Show the result, sketch why briefly, and do not belabour the proof** — this is a known result worth recognising, not deriving under exam conditions.

**Part C — merging two sorted lists.** Dummy head plus a `tail` pointer, take the smaller each time, attach the remainder at the end. Note the shape: *"This is the merge step of merge sort. Week 7."*

### Live-code (0:45–1:05)
Reversal from scratch (again — repetition is the point), then **LC 21 Merge Two Sorted Lists** with a dummy head.

### Guided practice (1:10–1:50)
1. **LC 206 Reverse Linked List** — Easy. Iterative, then recursive if they finish early (a good taste of Week 7).
2. **LC 21 Merge Two Sorted Lists** — Easy.
3. **LC 141 Linked List Cycle** — Easy.
4. **LC 234 Palindrome Linked List** — Easy/Medium. Composition: find the middle (fast/slow), reverse the second half, compare. **Three techniques in one problem** — call that out explicitly.

LC 234 is the session's real payoff. It is the first problem in the course that requires composing three separate learned techniques.

### Live critique (1:50–2:00)
An *LC 234*. Focus: did they restore the list afterwards? An interviewer will ask. Also: did they use O(n) space by copying to an array (valid, and worth stating as the simple solution) or O(1) by reversing in place?

### Flex (2:00–2:30)
**LC 142 Linked List Cycle II** — Medium. Find the cycle's starting node.

### Common misconceptions
- Reversal with the four lines in the wrong order — losing the list.
- Returning `cur` instead of `prev` from reversal.
- `while fast.next and fast` — order matters; this crashes.
- Using `==` instead of `is` for node identity.
- Forgetting that reversing the second half **mutates the caller's list**.

### Assignment 5.2 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 206 Reverse Linked List | Easy | 20 min |
| LC 21 Merge Two Sorted Lists | Easy | 25 min |
| LC 141 Linked List Cycle | Easy | 20 min |
| LC 234 Palindrome Linked List | Easy | 35 min |
| LC 142 Linked List Cycle II | Medium | 30 min |

---

## Session 3 (Thursday) — Pointer Surgery

### Warm-up (0:00–0:10)
1. Write the four lines of iterative reversal, in order.
2. Why does `while fast and fast.next` need both conditions?
3. Which three techniques does LC 234 combine?

### Concept spine (0:10–0:40)

**The general recipe for hard list problems**, stated as a procedure:

1. **Draw the before state.** Every node, every arrow.
2. **Draw the after state.**
3. **Identify which arrows changed** — usually two or three.
4. **Order the assignments** so nothing is lost before it is saved.
5. Add a dummy head if the first node might change.

**Part A — the gap technique (LC 19).** Remove the nth node from the end, in one pass. Advance `fast` n steps first, then move both until `fast` hits the end; `slow` is now n from the end. *"The gap between the pointers encodes the counting."*

Ask: *"Which node does `slow` need to be on to delete the target?"* → the one **before** it, which is exactly why the dummy head matters here.

**Part B — reorder (LC 143).** `1→2→3→4→5` becomes `1→5→2→4→3`. Three steps, all already known:
1. Find the middle (fast/slow)
2. Reverse the second half
3. Merge the two halves alternately

**Say this explicitly:** *"You already know all three pieces. Hard problems in interviews are usually compositions of easy ones — that is why we drill the templates."*

**Part C — copy with random pointers (LC 138).** Two approaches worth comparing:
- **Hash map:** old node → new node, two passes. O(n) time, O(n) space. Easy to reason about.
- **Interleaving:** weave copies into the original list (`A→A'→B→B'`), so `copy.random = node.random.next` falls out, then unweave. O(n) time, **O(1) extra space**. Clever and worth showing.

Presenting the simple solution first and then improving it is exactly what to do in an interview. Model it.

### Live-code (0:40–1:05)
**LC 19** with the gap technique and a dummy head, then **LC 143**, composing the three known pieces.

### Guided practice (1:10–1:50)
1. **LC 19 Remove Nth Node From End of List** — Medium
2. **LC 143 Reorder List** — Medium
3. **LC 138 Copy List with Random Pointer** — Medium

Three problems only; each is substantial.

### Live critique (1:50–2:00)
An *LC 19*. Focus: dummy head or special case? What happens when n equals the list length (removing the head)? That is the test case that separates working solutions from lucky ones.

### Flex (2:00–2:30) — Week 15 preview
**LRU cache, conceptually.** Do not implement it. Ask: *"You need get and put in O(1). A hash map gives O(1) lookup but no ordering. A doubly linked list gives O(1) removal but no lookup. What if you used both?"*

Let them sit with it. Then: *"That is LC 146, and it is one of the most-asked interview questions in existence. We build it in Week 15."* Planting it now makes Week 15 feel like a payoff.

### Common misconceptions
- In LC 19, stopping `slow` on the node to delete rather than the one before.
- In LC 143, forgetting to terminate the final list (leaving a cycle).
- In LC 138, copying `random` pointers before all nodes exist.
- Not handling single-node or empty lists.

### Assignment 5.3 (2h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 19 Remove Nth Node From End of List | Medium | 30 min |
| LC 143 Reorder List | Medium | 40 min |
| LC 138 Copy List with Random Pointer | Medium | 45 min |

---

## Session 4 (Friday) — ARENA: Contest 3

**Mode:** Contest · 90 minutes

| Slot | Problem | Difficulty | Tests |
|---|---|---|---|
| P1 | LC 160 Intersection of Two Linked Lists | Easy | Two pointers on lists, fresh |
| P2 | LC 2130 Maximum Twin Sum of a Linked List | Medium | Middle + reverse + scan, composed |
| P3 | LC 2 Add Two Numbers | Medium | Dummy head + carry handling |
| P4 | LC 25 Reverse Nodes in k-Group | Hard | Reversal under constraint |

**Calibration:** P1 is the gimme. P2 rewards anyone who understood LC 234 on Wednesday — it is the same three moves. P4 is genuinely hard and will be solved by at most two students; that is intended.

### Reveal (1:45–2:00)
Explain **P4** properly. It is reversal (which everyone knows) applied k nodes at a time with the group boundaries stitched back together. The lesson: *"You already had every piece. The difficulty was bookkeeping, not insight — and bookkeeping is what drawing the diagram first solves."*

### Assignment 5.4 (2h)
| Task | Budget |
|---|---|
| Upsolve all unfinished contest problems | 60 min |
| Editorial on **LC 25 Reverse Nodes in k-Group** | 45 min |
| Update tracker and red list | 15 min |

---

## Weekend Set 5 (6h) — due Tuesday, Week 6

### New problems (4h)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 24 Swap Nodes in Pairs | Medium | 30 min |
| LC 92 Reverse Linked List II | Medium | 40 min |
| LC 61 Rotate List | Medium | 35 min |
| LC 86 Partition List | Medium | 35 min |
| LC 328 Odd Even Linked List | Medium | 35 min |
| LC 1721 Swapping Nodes in a Linked List | Medium | 35 min |

**LC 92** is reversal with boundaries — the direct precursor to the contest's LC 25. **LC 86** uses two dummy heads, which is a genuinely useful trick. **LC 1721** looks like it needs two passes and does not — the gap technique from Thursday finds both nodes in one.

### Spaced revision (1h) — from Weeks 4 and 2
| Problem | Source | Target time |
|---|---|---|
| LC 5 Longest Palindromic Substring | Week 4 | under 20 min |
| LC 49 Group Anagrams | Week 2 | under 15 min |

### Written editorial (1h)
**LC 138 Copy List with Random Pointer.**

Compare **both** approaches: the hash map from original node to copy (two passes, O(n) time, O(n) space), and the interleaving trick that weaves copies into the original list so `copy.random = node.random.next` falls out for free (O(n) time, **O(1) extra space**).

The observation to look for: *"The difficulty is that a random pointer may target a node that has not been copied yet. The hash map solves this by making the mapping available before the second pass; the interleaving solves it by making each copy findable from its original in O(1), without any map at all."*

A student who explains **why the random pointer is the hard part** has understood the problem. One who describes two algorithms without naming the difficulty has not.

---

## Instructor notes

### What usually goes wrong this week
- **The reference model does not land on Tuesday.** Expect it. The `b = a; b.val = 99` demonstration needs to be run live, on the projector, with two arrows drawn pointing at one box.
- **Students code without drawing.** Hand out paper on Tuesday and require a diagram before code for the whole week. Enforce it in guided practice.
- **Reversal is written from memory incorrectly** for at least two more weeks. Put it in the warm-up repeatedly.
- **OOP anxiety.** Some students will feel classes are a big new topic. Deflate it: *"A class is a way to keep related data together. You are using five features of it, and that is all you need for this course."*

### Watch list
Linked lists split a cohort more sharply than any Phase I topic, because they require a mental model rather than a technique. A student still confusing `node` and `node.next` by Thursday will struggle badly with trees in Week 9 — intervene now, not after Checkpoint 1.

### What to cut if you are behind
1. LC 707 (Tuesday flex) — tedious, and its value is fluency you can get elsewhere
2. LC 138 from Thursday — keep it in the assignment, drop the in-class treatment
3. LC 61 and LC 328 from the weekend set
4. The LRU preview — Week 15 works without it, though it is a shame to lose

**Never cut:** the reference-semantics demonstration, the four-line reversal derivation, or the requirement to draw before coding.
