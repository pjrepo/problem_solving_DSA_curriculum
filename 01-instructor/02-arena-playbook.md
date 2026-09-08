# Arena Playbook — Contests, Mocks & Checkpoints

Friday's session. Never new content. Three modes across the 16 weeks:

| Mode | Weeks | Count |
|---|---|---|
| **Contest** | 1 (diagnostic), 2, 3, 5, 7, 8, 10, 12, 13, 14 | 10 |
| **Mock interview** | 4, 9, 15 | 3 |
| **Checkpoint** | 6, 11, 16 | 3 |

---

# Part A — Contest Mode

## Why contests

Students who can solve a problem in 45 unpressured minutes routinely fail to solve it in 25 with someone watching. That gap is trainable, and the only way to train it is to create the pressure repeatedly. Nine contests over the term is enough to make the pressure ordinary.

## Setup (0:00–0:15)

- Machines ready, editors open, LeetCode logged in before the clock starts
- **Problems given as a printed/posted list**, not links to browse — you are testing problem solving, not navigation
- Rules restated every single time (below)
- Leaderboard on the projector: names down the side, four problem columns, filled in live

## The contest (0:15–1:45, 90 minutes)

**Four problems, ascending difficulty.** The standard shape:

| Slot | Difficulty | Purpose |
|---|---|---|
| **P1** | Easy, on this week's topic | Everyone should solve this. Confidence and a non-zero score for all 14. |
| **P2** | Easy/Medium, this week's topic | The week's core skill under time pressure. |
| **P3** | Medium, this week + one earlier topic | Tests transfer and retention. |
| **P4** | Medium/Hard | Solved by 2–4 students. Stretch target; genuinely optional. |

**P1 must be solvable by your weakest student.** A contest where someone scores zero teaches them nothing except that they are bad at this. Calibrate down if unsure.

## Rules

1. **Individual work.** No talking, no shared screens.
2. **No LeetCode discussion tab, no editorials, no search, no LLM assistance.** Language documentation is permitted.
3. **Submissions on LeetCode; "Accepted" is the score.** Partial credit does not exist here — that is the point.
4. **Scoring:** solve order matters. First to solve a problem gets a bonus point. Ties broken by total time.
5. **You may leave when finished**, but the leaderboard stays up.

## The reveal (1:45–2:00)

**Immediately.** Do not defer to next week — the problem is still live in their heads and that is when explanation sticks.

- Read the final leaderboard aloud. Name who solved what.
- For each problem, **3 minutes maximum**: the trigger signal, the intended approach, the complexity. Not a full walkthrough.
- Ask the first solver of P3 or P4 to explain their approach to the room. Public credit is a real motivator, and explaining under mild pressure is free interview practice.

## Flex (2:00–2:30)

Full editorial for whichever problem the fewest students solved.

## Friday's assignment (always the same)

**Upsolve every problem you did not finish, plus write one editorial.** This is what converts a contest into learning. A contest without upsolve is just a scoreboard.

## Handling the leaderboard humanely

The public leaderboard is a strong motivator and a real risk with a cohort of 14. Manage it:

- **Post movement, not just position.** "Biggest improvement since Contest 3" is a column worth having, and it gives the bottom half something winnable.
- **Never comment negatively on a low score in public.** Handle it in a one-to-one.
- Track each student against **their own** previous contests — that curve is the real measure, and it is the one to show them privately.
- If a student is bottom for three consecutive contests, that is an intervention trigger, not a leaderboard event.

---

# Part B — Mock Interview Mode

Three rounds: **Week 4** (peer, gentle), **Week 9** (peer, real), **Week 15** (instructor-conducted, full FAANG format).

## Why this exists

Most students who fail DSA rounds can solve the problem. They fail because they start coding in silence, never clarify the input, cannot explain why their solution works, and freeze when the interviewer asks "can you do better?" None of that is fixed by solving more problems. It is fixed by rehearsal.

## Peer format (Weeks 4 and 9)

**7 pairs, two rounds of 45 minutes.** Every student interviews once and is interviewed once.

| Time | Activity |
|---|---|
| 0:00–0:10 | Brief: restate the rubric and the interviewer's job. Hand out problem cards. |
| 0:10–0:55 | **Round 1** — A interviews B |
| 0:55–1:05 | Swap, reset |
| 1:05–1:50 | **Round 2** — B interviews A |
| 1:50–2:00 | **Group debrief** — the failure patterns you saw |

**Pairing:** pair across ability, not within it. The stronger student gets practice explaining and evaluating; the weaker gets a competent interviewer. Re-pair differently in Week 9.

**Problem cards.** Prepare 7 cards, each with: the problem, 2–3 clarifying questions the interviewer should wait to be asked, the expected optimal approach and complexity, one hint to give if the candidate stalls past 15 minutes, and one follow-up ("now what if the array is sorted?"). Interviewers get the card; candidates see only the problem statement.

**The interviewer's job**, stated explicitly to them:
- Read the problem aloud, then stop talking
- **Do not confirm the approach** — ask "why does that work?" even when it is right
- Give the hint only after 15 minutes of no progress
- Take notes against the rubric throughout
- Ask the follow-up if there are 10 minutes left
- **Do not solve it for them.** Sitting through someone's silence is part of the exercise.

**Your job:** rotate continuously, spending ~6 minutes per pair, scoring the *candidate* against the rubric in `03-assessment-and-rubrics.md`. You will not see every minute of every interview — score what you see and supplement with the interviewer's notes.

## Instructor format (Week 15)

Full FAANG simulation. **14 students × 45 minutes** does not fit in one session — schedule these across the week, using Week 15's flex blocks and office hours, with the Friday session as the anchor and debrief.

Run it properly:
- Video-call format if possible, or a shared editor with **no autocomplete and no execution** — this is how most real rounds run
- You are a real interviewer: neutral affect, no encouraging nods, no confirmation
- One Medium, then a follow-up that extends it
- Score against the full rubric and give each student a written half-page: what would have passed, what would not

This is the highest-value hour you spend on any individual student all term.

## Group debrief — what to actually say

Do not review the problems. Review the **behaviours**. The recurring ones:

| Observed | What to say |
|---|---|
| Started coding within 30 seconds | "You lost points before you wrote a line. State your approach first, always." |
| Went silent for 8 minutes | "The interviewer cannot score silence. Narrate even when stuck — *especially* when stuck." |
| Never asked about constraints | "Ask the input size. It tells you the target complexity for free." |
| Said "it's O(n)" with no reasoning | "Justify it. Which loop, over what?" |
| Never tested the code | "Walk one example through, out loud, before you say you're done." |
| Panicked at "can you do better?" | "That question is not an accusation. It usually means there *is* a better solution — think out loud about what you're wasting." |

---

# Part C — Checkpoint Mode

Graded assessments in **Weeks 6, 11 and 16**.

| | Week 6 | Week 11 | Week 16 |
|---|---|---|---|
| **Covers** | Weeks 1–6 | Weeks 1–11 | Everything |
| **Format** | 3 problems, 2h, written complexity for each | 3 problems, 2h, mixed topics, unlabelled | Full mock loop: 2 problems, 45 min, verbalised |
| **Weight** | 7% | 8% | 10% |
| **Bar** | Solve 2 of 3 | Solve 2 of 3 | Would pass a real phone screen |

**Problems are unlabelled.** Students are not told which pattern applies — pattern *recognition* is the skill being assessed, and by Week 11 it is most of the skill.

**Grade against the code review rubric**, not just correctness. Return within one week with written feedback; with 14 students this is a 2-hour job and it is worth every minute.

**Checkpoints are diagnostic first.** After each one, update your intervention list. Week 6's real purpose is to find the students who will not survive Week 7 without help — see `05-intervention-guide.md`.
