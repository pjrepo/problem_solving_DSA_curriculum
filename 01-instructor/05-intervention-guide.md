# Intervention Guide

With 14 students, no one should reach Week 12 in trouble without you having caught it at Week 5. This document is how.

---

## 1. Early warning signals, in order of how early they fire

| Signal | Fires by | What it usually means |
|---|---|---|
| Day-zero *Two Sum* took >45 min or wasn't submitted | Week 1 | Weaker syntax fluency than assumed. Watch closely. |
| Silent during guided practice, screen barely changes | Week 2–3 | Stuck and not asking. The most common failure mode and the easiest to fix. |
| Editorials describe *what the code does*, not *why it works* | Week 3–4 | Pattern-matching without understanding. Will collapse at Week 7. |
| Assignments completed but complexity headers missing | Week 3–4 | Rushing to "done". Not internalising cost. |
| Bottom of leaderboard three contests running | Week 5–7 | Speed gap, or a foundational gap. Diagnose which. |
| Red list growing faster than it shrinks | Week 5–8 | Load exceeds capacity. Reduce before it compounds. |
| Cannot trace a recursive call stack on paper | **Week 7** | **The critical one. Act immediately.** |
| Checkpoint 1 below 12/25 | Week 6 | Foundation is not there; Phase II will not stick. |

---

## 2. Weekly triage (10 minutes, every Monday)

Open the tracker. For each of the 14, ask three questions:

1. **Did they submit the weekend set?** Two consecutive misses = flag.
2. **Was the editorial's "key observation" specific?** Vague for two weeks = flag.
3. **Is their red list growing?** Over 8 = flag.

Any flag means a **conversation this week**, not a note for later. Ten minutes of triage prevents the failure mode where a student quietly disengages in Week 6 and you discover it at Checkpoint 2.

---

## 3. Interventions by failure mode

### Mode A — "I don't know where to start"

The most common. The student can code but cannot select an approach.

**Do not give them the approach.** Give them the *procedure*:

1. What are the inputs, and what is the output? Write both down.
2. Solve one small instance **by hand, on paper**. What did you actually do?
3. Write the brute force. Any brute force. Get it working.
4. What is it recomputing?
5. What structure would make that lookup fast?

**The prescription:** for the next two weeks, they must write steps 1–3 on paper *before* opening the editor, for every problem. This feels slow to them; it is the fastest available fix, because the real deficit is that they have never had a procedure at all.

### Mode B — "I understood it in class and then couldn't do it at home"

Recognition without retrieval. They followed your derivation and mistook it for the ability to reproduce it.

**The prescription:** close the laptop after class and write the template from memory on paper. Compare, note the gaps, repeat next session. Add their weak templates to their personal red list. Two weeks of this fixes it.

### Mode C — "I can solve it but I run out of time"

A speed problem, not a knowledge problem. Usually caused by starting to code before deciding the approach, then rewriting twice.

**The prescription:** enforce a hard **5-minute think-before-typing rule** on every problem. They write the approach in a comment first. Counter-intuitively this makes them faster within about three weeks. Also: pull their contest problems and check whether they are re-deriving templates they should have memorised.

### Mode D — "I read the editorial after ten minutes"

Solution-reading masquerading as practice. Detected by a high solve count paired with weak contest performance and vague editorials.

**The prescription:** enforce the reproduce-from-scratch rule properly — read the editorial, close it, wait an hour, rewrite unaided. And reduce their volume: **6 well-understood problems beat 20 read ones.** Explicitly cut their weekend set in half for two weeks and tell them why.

### Mode E — The recursion wall (Week 7)

**Treat this as a different category. It is the single highest-risk moment in the course.**

The signature: they can write `factorial(n)` from memory but cannot say what `factorial(3)` leaves on the call stack, and cannot write a recursive function they have not seen before.

**The prescription, in this order:**

1. **Paper before code, for a full session.** Trace `factorial(4)` and `fib(5)` by hand, drawing every stack frame. No editor.
2. **The three-question drill** on every recursive function, forever: *What is the base case? What is the smaller subproblem? How do I combine the results?*
3. **Trust the leap.** The block is almost always disbelief that the recursive call will return the right answer. Make them write `# assume this returns the correct answer for n-1` as a literal comment. It works.
4. **Use the call stack they already know** from Week 6 — recursion is just a stack the language manages for you. This connection is why stacks are taught before recursion.
5. **Use Week 8's flex blocks for remediation** if needed. Falling a week behind is a far better outcome than proceeding to trees on a broken foundation.

**If more than 4 of 14 hit this wall, do not proceed as scheduled.** Re-teach with the whole class. Weeks 9, 13, 14 and 15 all sit on top of this.

### Mode F — Disengagement

Attendance slipping, submissions stopping, no contest participation. Often not about the course at all.

**The prescription:** a direct private conversation, early. Ask what changed. Common real causes: other course load, a demoralising contest result, or a conviction after one bad checkpoint that they cannot do this. All three are addressable — but only if you ask in Week 6 rather than Week 12.

---

## 4. What to do for the students who are ahead

Two or three students will find Phase I easy. Ignoring them is a real cost.

- **Give them the harder variant during guided practice** — every week file names one
- **Use them as interviewers** in mock rounds and as explainers during critique. Teaching is the strongest consolidation available, and it costs you nothing.
- **A parallel stretch list:** the Hard variant of each week's pattern, assigned instead of (not in addition to) two of the standard problems
- **Do not simply assign more volume.** They will burn out just as fast, and it will not make them better than depth would.

---

## 5. The load valve

This course asks ~24 hours a week. That is deliberate, and it will be too much for some students in some weeks.

**Cutting load is a legitimate instructional decision, not a failure.** In priority order, what to cut:

1. The flex block (always first)
2. The 4th problem in a daily set
3. The weekend set's new problems, from 6 down to 4 — **never** cut the revision hour or the editorial
4. For an individual student: halve the volume for two weeks and tell them explicitly it is temporary and deliberate

**Never cut:** the retrieval warm-up, the spaced revision hour, the written editorial, or Arena attendance. Those four are the load-bearing structure of the whole design.

### Predictable difficulty spikes

| When | What happens | Pre-empt it by |
|---|---|---|
| **Week 7** | Recursion wall | Warning them in Week 6 that this week is hard *by design*, and that struggling is normal |
| **Weeks 7–8** | Post-Checkpoint-1 slump | Returning CP1 with individual written feedback, framing it as diagnostic |
| **Week 11** | Graphs + Checkpoint 2 in one week | Lightening Week 11's daily assignments; the checkpoint is the load |
| **Weeks 13–14** | DP fatigue — six sessions of the hardest topic | Splitting DP across two weeks (already done), and making Week 13's contest generous |

---

## 6. The Week 6 decision point

Checkpoint 1 is the course's most important diagnostic, because it fires immediately before the recursion week.

After grading, sort the 14 into three groups:

| Group | CP1 score | Action |
|---|---|---|
| **Solid** | 18–25 | Proceed. Offer stretch problems. |
| **Shaky** | 12–17 | Proceed, but assign targeted revision on their specific weak pattern during Week 7's flex blocks. Watch them closely through the recursion week. |
| **At risk** | Below 12 | **Intervene before Week 7 starts.** A one-to-one this week. Identify whether the gap is hash maps, complexity, or general problem-solving procedure, and fix that specific thing. Do not let them enter recursion week with a Phase I hole. |

For the at-risk group, the honest conversation is worth having: *"You have a gap in X. If we fix it in the next two weeks you will be fine. If we don't, weeks 9 through 15 won't work for you. Here is the plan."* Second-year students almost always respond well to being told the truth early and given a concrete route out.
