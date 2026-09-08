# Student Handbook

*Problem Solving & DSA · 16 weeks · Leapstart School of Technology*

---

## What this course is

Sixteen weeks to take you from *"I can write a loop"* to *"I can pass a Big Tech DSA interview."*

By week 16 you should be able to read an unfamiliar LeetCode Medium, work out what kind of problem it is within three minutes, and produce clean working code in twenty — while explaining your reasoning out loud to someone watching.

You will solve about **320 problems**, sit **ten timed contests** and **three mock interview rounds**.

---

## The load — read this before you commit

| | Hours per week |
|---|---|
| Class (Tue/Wed/Thu/Fri) | ~9 |
| Daily assignments (4 × 2h) | 8 |
| Weekend assignment | 6 |
| **Total** | **~23** |

That is a lot, and it is deliberate. It is roughly the volume a self-directed candidate puts in before interviewing — except you get instruction, feedback and pressure-testing attached to it.

**You will hit a wall in Week 7, when we do recursion.** Almost everyone does. It is not a sign that you cannot do this; it is the normal shape of learning recursion after two years of writing loops. It passes.

---

## The weekly rhythm

| Day | What happens |
|---|---|
| **Tuesday** | Weekend set **due at the start of class**. New topic. 2h assignment. |
| **Wednesday** | New topic. 2h assignment. |
| **Thursday** | New topic. 2h assignment. |
| **Friday** | **The Arena** — timed contest, mock interview, or checkpoint. 2h assignment (upsolve + editorial). |
| **Sat–Mon** | 6h weekend set, submitted Tuesday morning. |

**The weekend set is always 4h new problems + 1h revision of older topics + 1h writing an editorial.** The last two hours are the ones that make the first four stick — do not skip them.

---

## The eight rules

**1. Struggle before you look. Then look properly.**
Every problem has a time budget. Stuck past it? Read the hint, then the editorial. **Then close it, wait an hour, and rewrite the solution from a blank file.** Reading a solution teaches nothing. Reproducing it unaided teaches a lot.

**2. Every solution states its complexity.**
In the header comment, time *and* space, before you move on. A solution without a stated complexity is not finished.

**3. Report your status honestly.**
Mark each problem *solved* / *needed a hint* / *read the editorial*. **"Read the editorial" is not a failure — it is data.** There is no penalty for needing help. There is a real penalty for hiding it: a student who marks everything "solved" and then freezes in a mock interview has wasted twelve weeks.

**4. Revision means re-solving, not re-reading.**
From a blank file, without looking at your old solution. If you look first you get the comfortable feeling of recognition and none of the benefit. Target: half the original time.

**5. Type your code. Do not copy it.**
Including during live coding in class. The templates need to be in your fingers by week 16, and they will not get there through your clipboard.

**6. Do not paste code you do not understand.**
Editorial and LLM assistance is allowed **after** your time budget expires and **only** if you then rewrite from scratch, unaided. Live code critique and mock interviews make undigested code obvious within minutes — but the real cost is yours, not the mark.

**7. Friday attendance is non-negotiable.**
Contests and mocks cannot be made up asynchronously. Performing under observation *is* the exercise.

**8. Say something before you fall behind.**
There are fourteen of you. Your professor will notice. It is far cheaper if you speak first.

---

## Your repository

```
dsa-<yourname>/
├── README.md                 your name, chosen language, your goal in one line
├── progress-tracker.md       copied from 02-progress-tracker-template.md
├── week-01/
│   ├── tue/  wed/  thu/  fri/
│   └── weekend/
│       ├── problems/
│       └── editorial.md
└── week-02/ ...
```

**Every solution file starts with this header. It is graded.**

```python
# LC 3 — Longest Substring Without Repeating Characters
# https://leetcode.com/problems/longest-substring-without-repeating-characters/
# Approach: variable sliding window; shrink from the left while a character repeats.
# Time: O(n)   Space: O(min(n, alphabet))
# Status: solved in 22 min
```

One commit per problem, not one per week.

---

## What a written editorial must contain

About 400–600 words. **Not a solution dump — a reasoning document.**

1. **Restate the problem** in your own words, with the constraints
2. **The brute force**, and its complexity
3. **The key observation** — the actual insight that unlocks the better solution
4. **The approach**, in prose, before any code
5. **Why it is correct** — an argument, not "it passed the tests"
6. **Time and space complexity**, justified
7. **What you would say in the first 60 seconds** if an interviewer handed you this

**Item 3 carries the most marks and item 7 is the one people skip.** Item 3 is also how your professor can tell whether you understood the problem or matched a shape — so write it carefully, in your own words.

---

## How you are graded

| Component | Weight |
|---|---|
| Daily assignments | 20% |
| Weekend sets (including editorials) | 25% |
| Arena contests | 15% |
| Mock interviews | 15% |
| Checkpoints (weeks 6, 11, 16) | 25% |

**30% of your grade is for things other than getting the right answer.** Contests reward speed; mocks reward communication. Those are what actually separate candidates who pass interviews from candidates who can solve the problems at home.

Contest marks are **60% participation and completing the upsolve, 40% your trend against your own earlier contests** — not your position on the leaderboard. Going from one problem to three over ten contests scores well.

---

## How to actually study this

**Do this:**
- Draw before you code. Especially linked lists, trees and any pointer manipulation.
- Spend 5 minutes deciding the approach before typing. It feels slower and makes you faster within three weeks.
- Read the constraints first — they tell you the intended complexity for free.
- After solving, ask: *"what pattern was that, and what phrase in the statement would have told me?"* That question is the whole course.
- Keep your red list honest and work it down.

**Not this:**
- Solving 30 problems you half-understand instead of 10 you fully do
- Reading solutions after 10 minutes
- Skipping the editorial because "I already solved it"
- Skipping revision because the topic feels old — that is exactly when it is decaying
- Going quiet when you fall behind

---

## Your red list

Any problem that is **not yet learned** goes on it:
- You read the editorial
- You needed a hint
- On revision it took as long as the first time
- It came up in a contest or checkpoint and you did not solve it

It comes off only when you solve it cleanly, unaided, on a later attempt. **Your weekend revision hour draws from your own red list first.**

If your red list passes 8 items, tell your professor. It means you are accumulating debt faster than you are repaying it, and the fix is to reduce volume and go deeper — not to push harder.

---

## The three checkpoints

| | Week | Covers | Format |
|---|---|---|---|
| Checkpoint 1 | 6 | Weeks 1–6 | 3 problems, 2h, unlabelled |
| Checkpoint 2 | 11 | Weeks 1–11 | 3 problems, 2h, unlabelled |
| Final Assessment | 16 | Everything | 2 problems, 90 min, plus a recorded verbal walkthrough |

**Problems are unlabelled** — you are not told which pattern applies. Working that out *is* the skill being assessed.

---

## The three mock interviews

| | Week | Format | Passing bar |
|---|---|---|---|
| Round 1 | 4 | Peer, Easy problems | 15 / 30 |
| Round 2 | 9 | Peer, Medium problems | 20 / 30 |
| Round 3 | 15 | **Instructor, full FAANG format** | **23 / 30 — a real phone-screen pass** |

**Only 10 of the 30 points are for the code.** The rest is for clarifying the problem, stating your approach before coding, narrating while you work, testing your own solution, and justifying your complexity.

Expect Round 1 to go badly. That is what it is for.

---

## One last thing

The single strongest predictor of how this course goes for you is not how quickly you pick things up in week 2. It is whether you keep going in weeks 7 and 13, when it gets hard.

Everyone struggles somewhere in this. The students who come out able to pass interviews are the ones who said so early and kept working.
