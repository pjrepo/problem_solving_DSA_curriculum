# Spaced Revision System

The mechanism that stops Week 2 from evaporating by Week 10.

---

## 1. The problem this solves

A student solves *Two Sum* in Week 2 and never touches a hash-map problem again until Week 11's graph adjacency lists. By then the pattern is gone. They will re-derive it slowly, or not at all, and in an interview they will not recognise it.

This is not a motivation problem or an intelligence problem — it is how memory works. Without deliberate re-exposure, retention of a solved-once technique decays sharply within weeks. The counter is **spaced retrieval**: revisit at expanding intervals, and each retrieval makes the next one last longer.

Everything below is designed to be **mechanical**. You should never have to decide what to revise.

---

## 2. The rule

> **Every weekend set's revision hour pulls 2 problems from weeks N−1, N−3 and N−6.**

The same spacing drives the daily **retrieval warm-up** questions.

| Current week | Revision draws from |
|---|---|
| 7 | 6, 4, 1 |
| 8 | 7, 5, 2 |
| 9 | 8, 6, 3 |
| 10 | 9, 7, 4 |
| 11 | 10, 8, 5 |
| 12 | 11, 9, 6 |
| 13 | 12, 10, 7 |
| 14 | 13, 11, 8 |
| 15 | 14, 12, 9 |
| 16 | 15, 13, 10 |

**Weeks 2–6** cannot use the full window because the source weeks do not exist yet. The rule for the early weeks:

| Week | Window that exists | What it draws from |
|---|---|---|
| 2 | — | Week 1, as a **timed baseline** rather than revision |
| 3 | N−1 only | Week 2 (both problems) |
| 4 | N−1, N−3 | Weeks 3 and 1 |
| 5 | N−1, N−3 | Weeks 4 and 2 |
| 6 | N−1, N−3 | Weeks 5 and 3 |

The full three-way spacing starts at **Week 7**.

**Week 16** substitutes its whole weekend mixed set for the revision hour — by then every problem is revision, and the set is chosen from wherever the cohort is weakest.

Each week file names its revision problems explicitly, so this requires no decisions during term.

### Two problems, three source weeks

Pick the two that best cover different patterns. The selection is already made in each week file, following this priority:

1. A problem the cohort struggled with the first time (from your red list)
2. A pattern not otherwise reinforced since it was taught
3. Something whose trigger signal is easily confused with the current week's topic — this is *interference training*, and it is the most valuable kind

---

## 3. Re-solve, don't re-read

State this to students explicitly and repeat it:

> Revision means **solving the problem again from a blank file**, without looking at your old solution. If you look first, you get the comfortable feeling of recognition and none of the benefit.

Target: a revision problem should take **under half** the original time. If it takes as long as the first attempt, the pattern was never learned — it goes on the red list.

Students record revision attempts in `progress-tracker.md` with the time taken, which makes the improvement visible and makes the red list self-populating.

---

## 4. The red list

Per student, a running list of problems that are **not yet learned**. A problem lands on the red list when:

- They read the editorial rather than solving it
- They needed a hint
- On revision, it took as long as (or longer than) the first attempt
- It appeared in a contest or checkpoint and they did not solve it

**It leaves the red list only when they solve it cleanly, unaided, on a later attempt.**

### Rules

- Red list lives at the bottom of each student's `progress-tracker.md`
- **The weekend set's revision hour draws from the student's own red list first**, then from the week's scheduled problems. This personalises revision at zero cost to you.
- **Red list over 8 items = flag.** They are accumulating debt faster than they are repaying it, and the load needs reducing before it compounds.
- Review the red lists when reading editorials each weekend. Patterns across students tell you what to re-teach in the next warm-up.

---

## 5. The interference principle

The most useful revision problems are the ones **easily confused with what you are teaching now**. Recognition of a pattern in isolation is easy; discrimination between similar patterns is the actual interview skill.

Deliberate confusable pairs, built into the week files:

| Teaching | Revise this alongside | The discrimination being trained |
|---|---|---|
| Sliding window (Wk 3) | Two pointers opposite-end (Wk 3) | Same direction vs. converging |
| BFS (Wk 11) | Level-order tree traversal (Wk 9) | It is the same algorithm — see it |
| DP (Wk 13) | Greedy (Wk 12) | When does the locally-best choice fail? |
| Backtracking (Wk 15) | DFS (Wk 11) | Backtracking is DFS that undoes its moves |
| Binary search on answer (Wk 8) | Binary search on array (Wk 8) | What is the search space? |
| Union-Find (Wk 12) | DFS connected components (Wk 11) | Both find components; when is each right? |

By Week 16 students should be able to look at an unlabelled problem and say *why it is not* the pattern it superficially resembles. That is what Week 16's Tuesday pattern-recognition drill assesses.

---

## 6. Warm-up question bank

Keep one file, appended to as you go. Format:

```
[Wk 3 · sliding window] Longest substring with at most K distinct characters —
  what's in the window, and what makes you shrink it?
[Wk 6 · monotonic stack] Why does the stack stay decreasing in Next Greater Element?
[Wk 9 · BST] Why does inorder traversal of a BST come out sorted?
```

Tag each with its source week so you can pull the N−1 / N−3 / N−6 set in seconds on the morning of a session. After a few weeks you will have a bank of 60+ and warm-up prep drops to a two-minute job.
