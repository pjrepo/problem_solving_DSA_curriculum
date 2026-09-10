# Optional Practice — How to Use This

Four tracks per session, for all 64 sessions of the course. **None of it is graded. None of it is expected.**

---

## Read this before you open any week file

> ### Do not open these files if:
>
> - You are **behind on the standard set**. Finish Tuesday's assignment before you look at Tuesday's optional problems.
> - You have **8 or more unresolved items on your red list**. That threshold is in your progress tracker for a reason: *"You are accumulating debt faster than you are repaying it, and the fix is to reduce volume and go deeper — not to push harder."* This file is more volume. It is the wrong tool for that problem.
> - You are in **Week 7** and recursion has not landed yet. Re-solve Week 7's standard problems instead. Nothing in here will help and the extra load will hurt.

The instructor guide is blunt about this, and it applies to this section more than anything else in the repo:

> **Do not simply assign more volume.** They will burn out just as fast, and it will not make them better than depth would.
> — `01-instructor/05-intervention-guide.md`, §4

**This set exists to be selected from, not completed.** Nobody should ever finish a session's optional block. Pick the one track that matches where you actually are that day, do one or two problems from it, and stop. A student who does 40 optional problems across the whole term has used this correctly. A student who tries to do all 384 has misunderstood it, and will be worse off than the student who did none.

---

## It is not graded

Not in the 100%. Not submitted. Not tracked. It does not appear in `01-instructor/03-assessment-and-rubrics.md` and it never will.

You can log optional problems in your progress tracker if you find it useful — but keep them out of your **red list**. The red list is for problems you were *assigned* and have not yet learned. Putting optional problems in it will make your debt look worse than it is and trigger an intervention you do not need.

---

## Picking a track

Each session has the same four tracks. Choose by how the standard assignment actually went — using the honest status codes you already use in your tracker (`S` solved unaided · `H` needed a hint · `E` read the editorial).

| If the standard set was… | Go to | Why |
|---|---|---|
| Mostly `E` — you read editorials | **1. Reinforce** | More reps at a difficulty you can actually finish. Confidence and fluency, not new ideas. |
| Mostly `S`, and quick | **2. Stretch** | One band harder than the week. This is the track for the two or three of you who finish early. |
| Fine today, but you have forgotten last month | **3. Revision** | Problems on older patterns, drawn from weeks N−1, N−3 and N−6. |
| Fine, but you freeze when someone watches | **4. Interview follow-ups** | No code. The questions an interviewer asks *after* you get it working. |

**Mostly `H` — you needed hints but got there?** That is the healthy middle of the cohort and the honest answer is: do nothing extra. Go and re-solve today's problems from a blank file tomorrow instead. That is worth more than anything on these pages.

---

## How difficulty works here

**Bands are relative to the week, not absolute.**

- **Reinforce** sits one band *below* the week's standard set.
- **Stretch** sits one band *above* it.

So Reinforce in Week 2 is Easy, while Reinforce in Week 15 is Medium. A "Medium" in the Week 3 Stretch track is a gentler problem than a "Medium" in the Week 14 Stretch track, because the surrounding week is different. Compare a problem to the week it sits in, not to the label.

### The Hard in every Stretch track

Every session — all 64, including Week 1 — has a **Hard** in its Stretch track. In Phase I this is deliberately outside the phase's difficulty band (`00-overview/03-topic-map.md`, §1, puts Phase I at Easy → Easy/Medium). Those early Hards are *toolkit* Hards — heavy hash-map and string work — not algorithmic ones, because Week 1 teaches no algorithms by design.

**Treat a Phase I Hard as a curiosity, not a target.** If you do not solve it, that is the expected outcome and it means nothing about you. Read the editorial without guilt; it is not on your red list because it was never assigned.

---

## Problem numbers: how these were checked

Every problem in this section was generated against **LeetCode's live problem list**, fetched on 2026-09-10. For all 384 entries the following were checked programmatically and all passed:

- the problem number **exists**;
- the **title matches** the number exactly;
- the stated **difficulty matches** LeetCode's;
- the problem is **not Premium**;
- the problem is **not already assigned** anywhere in `02-weeks/` (all 384 are new to the curriculum);
- no problem appears **twice** across the 64 sessions.

Problems were also constrained to techniques the course has **already taught by that week**, so nothing in a Reinforce, Revision or Stretch-Medium slot depends on material from a later week. The one deliberate exception is the **Stretch Hard**, which is allowed to reach ahead — that is what the difficulty note above is about.

Topics the curriculum explicitly excludes (`00-overview/03-topic-map.md`, §6) were filtered out: no segment trees, Fenwick trees, MST, suffix structures, or KMP/Rabin–Karp problems appear here.

### What was *not* checked

- **Nothing was solved.** Time budgets are estimates by difficulty band, not measured.
- **Topical fit was checked by tag, then spot-corrected by hand.** A handful of problems sit at the edge of their session's topic; where that is deliberate it is noted in the session itself.
- **LeetCode numbers can still drift**, and titles remain the reliable identifier — `03-reference/01-master-problem-index.md` makes the same point about the assigned curriculum. Re-run the checks before a future term rather than trusting a year-old list.
- **These problems are deliberately absent from the master problem index**, which indexes the *assigned* curriculum only. That is intentional: nothing here is assigned.

To re-check everything after editing this section, see **Verifying this section** at the end of this file.

## For instructors

- These files are safe to hand out wholesale, or to withhold entirely. Nothing in `02-weeks/` depends on them.
- The **Stretch** track is the closest thing here to the "parallel stretch list" in `05-intervention-guide.md`, §4. If you use it that way, assign it **instead of** two problems from the standard set, not on top of them — that is what the intervention guide actually prescribes, and it keeps the week at ~23 hours.
- The **Reinforce** track is the one to point a struggling student at, but only after you have checked that the gap is fluency rather than a hole from three weeks ago. If it is a hole, `05-intervention-guide.md` is the right file, not this one.
- **Check your understanding** questions have no answer key. That is deliberate — the same reason the retrieval warm-up runs cold in `01-instructor/01-session-playbook.md`. Printing the answer next to the question converts retrieval practice into reading, which is worth roughly nothing. They also make serviceable warm-up questions if you want extras.

---

## Verifying this section

Four checks, all runnable from the repo root. They confirm internal consistency; they cannot confirm that a problem is *pedagogically* right for its slot.

```bash
# 1. no optional problem collides with the assigned curriculum
grep -h '^| LC ' 05-optional-practice/week-*.md | grep -ohE '^\| LC [0-9]+' | sed 's/| LC //' | sort -u > /tmp/opt.txt
grep -ohE 'LC [0-9]+' 02-weeks/*.md 03-reference/*.md | sed 's/LC //' | sort -u > /tmp/assigned.txt
comm -12 /tmp/opt.txt /tmp/assigned.txt          # must be empty

# 2. no problem used twice inside this section
grep -h '^| LC ' 05-optional-practice/week-*.md | grep -ohE '^\| LC [0-9]+' | sort | uniq -d   # must be empty

# 3. every session has all four tracks and a Hard
for f in 05-optional-practice/week-*.md; do
  echo "$(basename $f) $(grep -c '^## Session' $f) $(grep -c '^### 1\. Reinforce' $f) \
$(grep -c '^### 2\. Stretch' $f) $(grep -c '^### 3\. Revision' $f) \
$(grep -c '^### 4\. Interview' $f) $(grep -c '| \*\*Hard\*\* |' $f)"
done                                              # every row: 4 4 4 4 4 4

# 4. session titles still match the week files
for n in $(seq -w 1 16); do
  diff <(grep '^## Session' 02-weeks/week-$n.md) \
       <(grep '^## Session' 05-optional-practice/week-$n.md) >/dev/null || echo "MISMATCH week-$n"
done
```

To re-verify numbers, titles, difficulty and Premium status against LeetCode, fetch `https://leetcode.com/api/problems/all/` and compare each table row against it.
