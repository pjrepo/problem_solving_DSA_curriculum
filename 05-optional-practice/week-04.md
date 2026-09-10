# Week 4 — Strings & Matrices · Optional Practice

**Phase I** · Week band: **Medium** · Reinforce = Easy · Stretch = Medium/Hard

> Optional. Not graded. Read `00-how-to-use-this.md` first.
>
> Friday is the **first mock interview**. The Interview follow-up track matters more this week than any other so far — it is the only track that rehearses the thing Friday actually tests.

---

## Session 1 (Tuesday) — Strings & Palindromes

> Optional. Not graded. Skip freely.

### Check your understanding
1. A palindrome check from both ends is O(n). What does the *expand-around-centre* approach buy you that the two-pointer check does not?
2. Why are there `2n − 1` centres to expand around, not `n`?
3. You are asked for the longest palindromic *substring*. Why does sorting or a hash map not help here at all?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 13 Roman to Integer | Easy | 20 min |
| LC 58 Length of Last Word | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 8 String to Integer (atoi) | Medium | 40 min |
| LC 65 Valid Number | **Hard** | 55 min |

LC 8 and LC 65 are the two most notorious "easy-sounding, brutal-in-practice" string problems on the site. Neither needs an algorithm; both punish sloppy case analysis, which is exactly the skill Friday's mock will test.

### 3. Revision — Week 3 (Two Pointers & Window) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1768 Merge Strings Alternately | Easy | 25 min |
| LC 31 Next Permutation | Medium | 35 min |

### 4. Interview follow-ups
1. **"What if the string contains Unicode rather than ASCII?"** — A good answer separates *bytes* from *characters* and says which one the index arithmetic assumes. Then it notes that a 26-slot array is no longer valid and a hash map is needed.
2. **"Can you do the palindrome check without extra space?"** — A good answer gives the two-pointer O(1)-space version and contrasts it with reversing the string, which is O(n) space for no gain.

---

## Session 2 (Wednesday) — Sliding Window on Strings

> Optional. Not graded. Skip freely.

### Check your understanding
1. A window over a string with "at most K distinct characters" — what is the state you carry, and what triggers the shrink?
2. Why does the answer for a *variable* window get recorded after the shrink loop, not before it?
3. Anagram-in-a-string is a *fixed* window. What makes it fixed, and what is the cheap way to compare the two frequency maps at each step?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 412 Fizz Buzz | Easy | 20 min |
| LC 415 Add Strings | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 12 Integer to Roman | Medium | 40 min |
| LC 564 Find the Closest Palindrome | **Hard** | 55 min |

LC 564 is a case-analysis problem wearing a maths costume — worth it for the discipline of enumerating candidates exhaustively before coding.

### 3. Revision — Week 3 (Two Pointers & Window) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 455 Assign Cookies | Easy | 25 min |
| LC 2149 Rearrange Array Elements by Sign | Medium | 35 min |

### 4. Interview follow-ups
1. **"Your window uses a hash map. Could you use an array instead?"** — A good answer ties it to the alphabet: 26 lowercase letters means a fixed array and a much better constant factor. It also says the big-O is unchanged, which is the honest part most candidates skip.
2. **"How do you know the window never needs to grow backwards?"** — A good answer states the monotonicity: once a left edge is invalid it stays invalid, so left never decreases. That is the argument for O(n) and the reason the technique works at all.

---

## Session 3 (Thursday) — Matrices & Grid Thinking

> Optional. Not graded. Skip freely.

### Check your understanding
1. Rotating a matrix in place: which two simpler operations compose into a 90° rotation?
2. For a spiral traversal, what are the four boundaries you maintain, and what is the condition that stops the loop?
3. You need to mark rows and columns for clearing. Why is doing it with a set of indices O(m + n) space, and where would you store the marks to get O(1)?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 1572 Matrix Diagonal Sum | Easy | 20 min |
| LC 566 Reshape the Matrix | Easy | 25 min |

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 498 Diagonal Traverse | Medium | 40 min |
| LC 1074 Number of Submatrices That Sum to Target | **Hard** | 55 min |

### 3. Revision — Week 3 (Two Pointers & Window) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 2215 Find the Difference of Two Arrays | Easy | 25 min |
| LC 1423 Maximum Points You Can Obtain from Cards | Medium | 35 min |

### 4. Interview follow-ups
1. **"Can you do it without the extra O(m + n) marker space?"** — A good answer uses the first row and column as the marker storage and flags the one cell that overlaps as the special case. It also admits that this trades clarity for space and asks whether that is wanted.
2. **"What if the matrix were not square?"** — A good answer notes that in-place rotation stops being possible — the shape changes — so you must allocate. Recognising when in-place is *impossible* is worth as much as knowing the trick.

---

## Session 4 (Friday) — ARENA: Mock Interview Round 1

> Optional. Not graded. Skip freely.
>
> Your actual Friday assignment is upsolve + editorial. **Do that first.**

### Check your understanding
1. Mock day. Before you look at anything: what are the first three things you say out loud when handed a problem?
2. Your interviewer says nothing for two minutes while you think. What should you be doing with that silence?
3. You finish coding with ten minutes left. What do you do with them?

### 1. Reinforce — if the standard set was hard
| Problem | Difficulty | Budget |
|---|---|---|
| LC 171 Excel Sheet Column Number | Easy | 20 min |
| LC 1108 Defanging an IP Address | Easy | 25 min |

Both are five-minute problems. Use them as warm-ups *before* your mock slot, not after — arriving cold is what causes the first ten minutes to go badly.

### 2. Stretch — if the standard set was easy
| Problem | Difficulty | Budget |
|---|---|---|
| LC 38 Count and Say | Medium | 40 min |
| LC 899 Orderly Queue | **Hard** | 55 min |

### 3. Revision — Week 3 (Two Pointers & Window) · Week 1 (Toolkit & Complexity)
| Problem | Difficulty | Budget |
|---|---|---|
| LC 953 Verifying an Alien Dictionary | Easy | 25 min |
| LC 1248 Count Number of Nice Subarrays | Medium | 35 min |

### 4. Interview follow-ups
1. **"Can you do better?"** — A good answer treats this as information, not criticism: it usually means a better solution exists. Say what your current solution wastes — a re-scan, a re-sort, a recomputation — and reason forward from the waste.
2. **"Walk me through your code with this input."** — A good answer traces the actual values out loud and *catches its own bug*. Doing this unprompted, before saying "I'm done", is the single biggest scoring difference between candidates at this level.
