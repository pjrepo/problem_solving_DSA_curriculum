# Week 1 — Toolkit & Complexity · Optional Practice

**Phase I** · Week band: **Easy** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first — particularly the part about not opening this when you are behind.
>
> **Week 1 note.** This week teaches no algorithms by design, so the Stretch Hards here are *toolkit* Hards: heavy string and hash-map machinery, not clever algorithms. They are far outside the week's band. Treat them as curiosities.

---

## Session 1 (Tuesday) — The Problem-Solving Frame & the Array/String Toolkit

> Optional. Not graded. Skip freely.

### Check your understanding
1. `sorted(arr)` and `arr.sort()` — which returns what? What is in `arr` after you write `arr = arr.sort()`, and why?
2. You need a fixed pair of coordinates usable as a dictionary key. Which of the four containers, and why does each of the other three fail?
3. Building a 100,000-character string with `+=` in a loop is slow. What is the actual mechanism that makes it slow, and what do you write instead?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1920 Build Array from Permutation | Easy | 15 min |
| LC 1662 Check If Two String Arrays are Equivalent | Easy | 20 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 165 Compare Version Numbers | Medium | 35 min |
| LC 68 Text Justification | **Hard** | 60 min |

LC 68 is nothing but careful string assembly and off-by-one discipline — no algorithm at all. It is a fair fight in Week 1 and it will take longer than you expect.

### 3. Revision — Week 1 has no prior weeks; this revises day-zero prerequisites
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1281 Subtract the Product and Sum of Digits of an Integer | Easy | 15 min |
| LC 1051 Height Checker | Easy | 20 min |

### 4. Interview follow-ups
1. **"Could you do that without allocating a new array?"** — A good answer distinguishes *can* from *should*: in-place is O(1) space, but it destroys the caller's input. Say which you chose and why, and ask whether the caller needs the original.
2. **"What does your function do on an empty input?"** — A good answer has already handled it before being asked. State your assumptions about the input as part of your approach, not after the interviewer finds the hole.

---

## Session 2 (Wednesday) — Hash Maps & Sets

> Optional. Not graded. Skip freely.

### Check your understanding
1. In one-pass Two Sum, what exactly is stored in the dictionary — and why is the value an index rather than the number itself?
2. Why can a tuple be a dictionary key but a list cannot? What is the underlying requirement?
3. `Counter` and `defaultdict(int)` both count. Name one thing each does that the other does not.

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 771 Jewels and Stones | Easy | 15 min |
| LC 1436 Destination City | Easy | 20 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 890 Find and Replace Pattern | Medium | 35 min |
| LC 149 Max Points on a Line | **Hard** | 55 min |

LC 890 needs *two* maps, not one — the mapping has to be a bijection. LC 149 is a hash map keyed on a slope, and the whole difficulty is representing that slope without floating-point error.

### 3. Revision — Tuesday's array/string toolkit
| Problem | Difficulty | Budget |
|---|---|---|
| LC 557 Reverse Words in a String III | Easy | 20 min |
| LC 66 Plus One | Easy | 25 min |

### 4. Interview follow-ups
1. **"Your solution uses O(n) extra space. Can you do it in O(1)?"** — A good answer names the trade rather than panicking: the hash map bought you time, and giving it up usually means sorting first (O(n log n) time, but O(1) extra) or exploiting a constraint on the value range. Ask whether the input can be modified.
2. **"What if the keys were lists rather than numbers?"** — A good answer goes straight to hashability: convert to a tuple, or to a canonical string. Mention that the conversion itself costs O(k) per key and that this changes your complexity.

---

## Session 3 (Thursday) — Time & Space Complexity

> Optional. Not graded. Skip freely.

### Check your understanding
1. Two loops over the same array, one after the other. Is that O(n) or O(n²)? Now put one inside the other. What changed, and why?
2. `if x in my_list` inside a loop over that list. What is the true complexity, and what one change fixes it?
3. The constraint says `n ≤ 1000`. What complexity is the problem asking you for, and how do you know?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 645 Set Mismatch | Easy | 25 min |
| LC 414 Third Maximum Number | Easy | 25 min |

For both: write the obvious solution, state its complexity in the header, *then* ask whether you can do better. That order is the whole point of today.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 229 Majority Element II | Medium | 40 min |
| LC 862 Shortest Subarray with Sum at Least K | **Hard** | 60 min |

LC 229 is the direct sequel to today's LC 169 — and the O(1)-space version is a genuinely surprising argument. LC 862 is a preview of Week 6's monotonic deque; the O(n²) solution is easy to write and the point is being able to say precisely why it is too slow.

### 3. Revision — Tuesday's toolkit · Wednesday's hash maps and sets
| Problem | Difficulty | Budget |
|---|---|---|
| LC 228 Summary Ranges | Easy | 25 min |
| LC 202 Happy Number | Easy | 30 min |

LC 202 is a set used for cycle detection — the same idea returns as Floyd's algorithm in Week 5.

### 4. Interview follow-ups
1. **"You said that was O(n). Which loop, over what?"** — A good answer points at the specific line and names what n counts. "O(n) where n is the number of intervals, not the total length" is a real answer; "it's linear" is not.
2. **"Does your space complexity include the output?"** — A good answer states the convention it is using before giving the number. Output space is conventionally excluded; the interviewer wants to see that you know it is a convention rather than a fact.

---

## Session 4 (Friday) — ARENA: Diagnostic Contest

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.** These are for after it, or for the weekend if the weekend set is already done.

### Check your understanding
1. You had 90 minutes and four problems. What did you spend the most time on that was not writing code — and was that time well spent?
2. Which of the four would you now recognise instantly? Which one would you still not know where to start on?
3. Write down, from memory, the one-pass Two Sum. No looking. Did you get the dictionary update on the right line?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 500 Keyboard Row | Easy | 20 min |
| LC 599 Minimum Index Sum of Two Lists | Easy | 25 min |

Both are contest-shaped: read, recognise "set membership" or "hash map lookup", write it in five lines. That recognition speed is what the contest was measuring.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 274 H-Index | Medium | 35 min |
| LC 432 All O`one Data Structure | **Hard** | 60 min |

LC 432 is a design problem built entirely from Week 1's containers — no algorithm, just choosing the right combination of dict and linked structure. It is a real preview of Week 15.

### 3. Revision — the whole of Week 1
| Problem | Difficulty | Budget |
|---|---|---|
| LC 258 Add Digits | Easy | 15 min |
| LC 290 Word Pattern | Easy | 25 min |

### 4. Interview follow-ups
1. **"Walk me through your first 60 seconds on the problem you did not solve."** — A good answer restates the problem, names what the input and output are, and identifies the pattern it *looks* like — even if that guess turns out wrong. Silence for 60 seconds scores zero regardless of what comes after.
2. **"You solved this in the contest but you read the editorial for that one. What was the difference?"** — A good answer is specific about the missing piece — an unfamiliar structure, an insight you did not reach, or simply running out of clock. This is the single most useful question to ask yourself after every contest this term.
