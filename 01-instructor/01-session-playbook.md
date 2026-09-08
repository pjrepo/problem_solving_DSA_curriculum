# Session Playbook — Running a Lab Session

How to execute the Tue/Wed/Thu teaching sessions. The block structure is in `00-overview/02-weekly-rhythm.md`; this document is the *how*.

---

## Block 1 — Retrieval warm-up (0:00–0:10)

**Three questions on the board. Cold. No notes, no laptops.** Students write answers on paper; you reveal after 5 minutes and take 5 minutes to discuss.

Draw the questions from **weeks N−1, N−3 and N−6** — the same spacing as the weekend revision.

Good warm-up questions test *retrieval*, not recognition:

| Weak (recognition) | Strong (retrieval) |
|---|---|
| "What is a hash map?" | "You need to find if any two numbers in an unsorted array sum to K. What's the approach and its complexity?" |
| "What does Floyd's algorithm do?" | "Write the four lines that detect a cycle in a linked list." |
| "Is binary search O(log n)?" | "Binary search on `[1,3,5,7]` for 4. Where does `lo` end up, and why is that useful?" |

**Do not skip this block when running late.** Cut the flex block instead. Retrieval practice is the single highest-return use of ten minutes in this entire course, and its benefit is invisible week-to-week — it shows up in Week 11 when students still remember Week 2.

**On Tuesdays** this slot becomes the 15-minute weekend-set debrief: walk through the two problems most students struggled with, chosen while reading submissions that morning.

---

## Block 2 — Concept derivation (0:10–0:40)

**The rule: never present a technique as a finished object.** Derive it.

The shape that works, every time:

1. **State the problem.** A concrete instance with real numbers, on the board.
2. **Write the brute force.** Out loud, as a class. Get its complexity.
3. **Find the waste.** "We're recomputing the sum of this window every time. How many times do we add `arr[5]`?" Let the room answer.
4. **Invent the fix.** Guide them to it. The technique should feel *discovered*, not *given*.
5. **Name it.** *Now* say "this is called a sliding window." The name lands on something they already understand.
6. **Generalise the trigger.** "When you see 'contiguous subarray' plus 'longest/shortest/max', reach for this."

Step 6 is what converts a solved problem into a transferable pattern. Do not skip it — it is the difference between students who have seen 300 problems and students who can solve the 301st.

**Board discipline:** keep the brute force visible on one side of the board while you develop the optimisation on the other. The comparison is the lesson.

---

## Block 3 — Live-code the template (0:40–1:00)

**You type. From scratch. Every time.** Do not paste prepared code and walk through it — the value is in watching a working programmer make decisions in real time.

Rules:

- **Narrate every decision**, including trivial ones. "I'm calling this `left` not `i` because in a two-pointer problem `i` tells you nothing."
- **Make a deliberate mistake roughly every third session** — an off-by-one, a wrong initial value — and then debug it in front of them. Watching an expert *recover* is more instructive than watching one be flawless, and it removes the belief that good programmers write correct code first time.
- **Students type along.** No copy-paste. The canonical templates need to be in their fingers by week 16.
- **Call out the JavaScript delta** where it matters: `Map` vs object for non-string keys, no tuple unpacking, `sort()` comparing as strings by default, no integer division operator, no `heapq` in the standard library.
- **Stop at the template.** Do not extend into variants here — that is the flex block.

Every canonical template is in `03-reference/03-code-templates-python.md`. Teach exactly those forms so students see one consistent shape all term.

---

## Block 4 — Guided practice (1:10–1:50)

Students solve 2–3 problems in the lab. You circulate.

**With 14 students you can reach everyone twice in 40 minutes.** Use it:

- **First lap (~15 min):** look at screens, say nothing unless someone is truly stuck. You are gathering data on who is where.
- **Second lap (~25 min):** targeted intervention. Ask questions, don't give answers: *"What does your `left` pointer mean right now?"* · *"Walk me through this with `[1,2,3]`."* · *"What's the invariant you're maintaining?"*

**Never take the keyboard.** Point at the line, ask what it does, wait.

**Track it.** Keep a simple note per session of who was stuck and on what. Three sessions of the same student stuck early is your intervention trigger — see `05-intervention-guide.md`.

**When 5+ students are stuck on the same thing:** stop the room and re-teach it. That is not a failure of the students; it is a signal your derivation missed something.

---

## Block 5 — Live code critique (1:50–2:00)

One student's solution on the projector, reviewed by the room. **This is the block with the highest learning-per-minute in the session, and the one most likely to go wrong if handled carelessly.**

**Establish the norms in Week 1 and repeat them:**

- Everyone will be critiqued roughly four times this term. It is a rotation, not a punishment.
- We critique the **code**, never the coder. "This variable name doesn't say what it holds" — not "you named this badly."
- **The author speaks first**, explaining their approach. Often they find their own issue.
- **Start with what works.** Every solution has something right in it; say it before anything else.
- Working code that is hard to read is a **real** finding, not a nitpick. In an interview, unreadable code fails.

**Rotate deliberately.** Do not only show the best solutions — a partially-working solution with a good idea in it teaches more than a perfect one. And do not only show the strong students; being critiqued is how the middle of the cohort improves fastest.

**What to look for, in order:**
1. Correctness on edge cases (empty input, single element, all duplicates)
2. Complexity — does it match what they claimed?
3. Naming and structure — could a stranger read this in 30 seconds?
4. Idiom — are they using the language's tools, or fighting it?

---

## Block 6 — Flex block (2:00–2:30)

Only if you have the 2.5-hour session. **Cut this first when behind.** Each week file specifies its own flex content. Typical uses: a harder variant of the day's pattern, upsolving an earlier contest problem, or an awareness-level topic.

---

## Managing the spread

By Week 5 you will have a clear top three, a solid middle, and one or two who are struggling. All three groups need handling:

**The top:** give them the harder variant during guided practice, and use them as critique subjects for *advanced* discussion (not just as examples of correctness). Ask them to explain their approach to a neighbour — teaching is the best consolidation available.

**The middle:** this is where your circulation time should go. They usually do not need the concept re-taught; they need one specific misconception corrected.

**The struggling:** do **not** let them silently copy during guided practice. Sit with them for a full five minutes. Usually the block is not today's topic but something from three weeks ago — and that is worth finding out immediately. See `05-intervention-guide.md`.

---

## Session hygiene

- **Start on time**, even with 6 people in the room. It sets the norm within two weeks.
- **Write the session's one-line goal on the board** before students arrive: *"By the end of today you can find the longest substring without repeating characters in O(n)."*
- **Close with the trigger, not the summary.** Last thing they hear should be the pattern's *recognition signal* — that is what they need in an interview, not a recap.
- **Set the assignment before they leave**, in writing, with time budgets visible.
