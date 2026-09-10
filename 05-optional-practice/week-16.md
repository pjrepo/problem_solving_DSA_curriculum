# Week 16 — Simulation · Optional Practice

**Phase III** · Week band: **Medium/Hard** · Reinforce = Medium · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> The last week is consolidation and pattern recognition, and this file changes purpose to match: the problems here are **deliberately unlabelled by pattern** in the same spirit as the final assessment. Read each one and decide what it is before you write anything. That decision is the whole exercise now.
>
> Reinforce is **Medium** this week. Nothing here is new material.

---

## Session 1 (Tuesday) — Pattern Recognition Drill

> Optional. Not graded. Skip freely.

### Check your understanding
1. For each of these phrases, name the pattern in under five seconds: "contiguous subarray, longest" · "k largest" · "is it possible to order" · "count the ways" · "next greater" · "shortest number of steps".
2. Which two of the 29 patterns in the catalogue could you not currently write a template for from memory?
3. Given `n ≤ 10⁵`, which complexities are still on the table? And at `n ≤ 20`?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 204 Count Primes | Medium | 35 min |
| LC 636 Exclusive Time of Functions | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2657 Find the Prefix Common Array of Two Arrays | Medium | 40 min |
| LC 126 Word Ladder II | **Hard** | 55 min |

### 3. Revision — Week 15 (Backtracking · Bits · Design) · Week 13 (DP I) · Week 10 (Trees II · Heaps · Tries)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 461 Hamming Distance | Easy | 25 min |
| LC 1239 Maximum Length of a Concatenated String with Unique Characters | Medium | 35 min |

### 4. Interview follow-ups
1. **"You have three minutes. What kind of problem is this?"** — A good answer commits to a guess with a reason, and says what would change its mind. "I think sliding window because we want a contiguous range and a maximum; if the array had negatives I'd reconsider" is exactly the target behaviour.
2. **"How confident are you in that complexity?"** — A good answer distinguishes what it has proved from what it believes. Calibrated confidence reads as competence; overclaiming is caught immediately and costs more than admitting uncertainty.

---

## Session 2 (Wednesday) — Full Interview Simulation

> Optional. Not graded. Skip freely.

### Check your understanding
1. Full simulation day. Before your slot: what is your opening script, word for word?
2. What is your plan for the moment you get stuck for more than 90 seconds?
3. How do you test your code out loud, and how long does it take you?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2125 Number of Laser Beams in a Bank | Medium | 35 min |
| LC 2785 Sort Vowels in a String | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1647 Minimum Deletions to Make Character Frequencies Unique | Medium | 40 min |
| LC 301 Remove Invalid Parentheses | **Hard** | 55 min |

LC 301 is a Hard that rewards a clean BFS-over-strings insight rather than clever pruning — a good final exercise in choosing the simplest correct framing.

### 3. Revision — Week 15 (Backtracking · Bits · Design) · Week 13 (DP I) · Week 10 (Trees II · Heaps · Tries)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 832 Flipping an Image | Easy | 25 min |
| LC 1415 The k-th Lexicographical String of All Happy Strings of Length n | Medium | 35 min |

### 4. Interview follow-ups
1. **"Start whenever you're ready."** — A good answer does not start coding. It restates the problem, confirms input types and sizes, asks about edge cases, and states an approach with its complexity — and only then writes. Sixteen weeks of this course lead to that one habit.
2. **"Your solution is O(n²) and I'd like better."** — A good answer names the wasted work first, then proposes the structure that removes it. "I'm re-scanning the prefix each time, so if I keep a running count in a hash map that becomes O(n)" is the shape of every good optimisation answer.

---

## Session 3 (Thursday) — Weakness Clinic & the Roadmap

> Optional. Not graded. Skip freely.

### Check your understanding
1. Weakness clinic. Name your three weakest patterns, from your own progress tracker rather than from memory.
2. What is still on your red list, and what is the oldest item on it?
3. Which single pattern, if you fixed it this week, would change the most interview outcomes for you?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1026 Maximum Difference Between Node and Ancestor | Medium | 35 min |
| LC 538 Convert BST to Greater Tree | Medium | 40 min |

These are tree problems because trees recur in more interviews than anything else per hour spent. If your weak patterns are elsewhere, ignore these and go to that week's file instead — this is the one session where substituting from another week is explicitly the right call.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 669 Trim a Binary Search Tree | Medium | 40 min |
| LC 968 Binary Tree Cameras | **Hard** | 55 min |

### 3. Revision — Week 15 (Backtracking · Bits · Design) · Week 13 (DP I) · Week 10 (Trees II · Heaps · Tries)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 476 Number Complement | Easy | 25 min |
| LC 1642 Furthest Building You Can Reach | Medium | 35 min |

### 4. Interview follow-ups
1. **"What are you weakest at, and what are you doing about it?"** — A good answer is specific and unembarrassed: "DP on two sequences — I can do LCS but Edit Distance transitions still slow me down, so I'm re-solving that family this week." Vague self-deprecation reads worse than a named gap with a plan.
2. **"Where do you want to be in six months?"** — A good answer connects the gap to the plan without over-promising. This is not a DSA question and it is asked in almost every real loop.

---

## Session 4 (Friday) — FINAL ASSESSMENT

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Final assessment. Two problems, 45 minutes, verbalised. What does "would pass a real phone screen" mean in behaviour, not in score?
2. Sixteen weeks ago you had not used a hash map. From memory: name ten patterns and one problem each.
3. What is the first thing you will say when the interviewer stops talking?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2958 Length of Longest Subarray With at Most K Frequency | Medium | 35 min |
| LC 2348 Number of Zero-Filled Subarrays | Medium | 40 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2461 Maximum Sum of Distinct Subarrays With Length K | Medium | 40 min |
| LC 87 Scramble String | **Hard** | 55 min |

LC 87 is a Hard that is only tractable with memoisation over a well-chosen state — a fitting last problem, since choosing the state has been the recurring difficulty of Phase III.

### 3. Revision — Week 15 (Backtracking · Bits · Design) · Week 13 (DP I) · Week 10 (Trees II · Heaps · Tries)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1009 Complement of Base 10 Integer | Easy | 25 min |
| LC 1404 Number of Steps to Reduce a Number in Binary Representation to One | Medium | 35 min |

### 4. Interview follow-ups
1. **"Talk me through your thinking."** — A good answer has been doing this since minute one, so the question changes nothing. That is the actual exit bar of this course: a fresh Medium in 20–25 minutes, clean code, complexity justified aloud, without being asked.
2. **"Do you have any questions for me?"** — A good answer has two ready and specific. It is the last thing you will be asked in every real interview, and "no, I think you covered everything" is a wasted opportunity every single time.
