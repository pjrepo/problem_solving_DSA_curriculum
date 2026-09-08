# Day Zero — Setup & Ground Rules

Run this **before** Week 1 Session 1, either as a 30-minute pre-session or as a mandatory pre-read. If students spend Week 1's first hour installing Python, you have lost an irreplaceable hour.

---

## 1. Environment

### Python (primary)

- **Python 3.10 or later.** Verify: `python3 --version`
- Editor: **VS Code** with the Python extension. Uniformity matters — when you say "press F5", it should work for all 14.
- No packages needed. The standard library (`collections`, `heapq`, `bisect`, `functools`, `math`) covers the entire course.

### JavaScript (secondary)

- **Node 18+.** Verify: `node --version`
- Students may solve in JavaScript, but **must pick one language and stay with it** for at least the first eight weeks. Switching languages mid-course while also learning DSA means learning neither.

Python is the recommended default: less syntax noise, and the interview standard at most companies.

### Accounts

- A **LeetCode account** (free tier is sufficient for everything assigned)
- A **GitHub account** with one repository named `dsa-<yourname>`, shared with the instructor

---

## 2. Repository structure

Set this up on day zero. Consistency makes review possible.

```
dsa-<name>/
├── README.md              # your name, chosen language, one line on your goal
├── progress-tracker.md    # copy from 04-student/02-progress-tracker-template.md
├── week-01/
│   ├── tue/
│   ├── wed/
│   ├── thu/
│   ├── fri/
│   └── weekend/
│       ├── problems/
│       └── editorial.md
├── week-02/
└── ...
```

**Every solution file begins with this header.** No exceptions — it is graded.

```python
# LC 1 — Two Sum
# https://leetcode.com/problems/two-sum/
# Approach: one-pass hash map storing value -> index; look up complement.
# Time: O(n)   Space: O(n)
# Status: solved in 18 min / needed a hint / read the editorial
```

That last line is the important one. **Honest self-reporting is worth more than a clean record**, and the whole intervention system depends on it.

---

## 3. Ground rules

**1. Struggle before you look. Then look.**
The time budget on each problem is the contract. Stuck past it? Read the hint, then the editorial — but then **close it, wait an hour, and rewrite the solution from scratch**. Reading a solution teaches nothing; reproducing it teaches a lot.

**2. Every solution gets a complexity statement.**
Written in the header, before you move on. A solution without a stated complexity is not finished.

**3. Report your status truthfully.**
"Read the editorial" is not a failure — it is data. A student who marks everything "solved" and then freezes in a mock interview has wasted twelve weeks. There is no penalty for needing help; there is a real penalty for hiding it.

**4. Do not paste solutions you do not understand.**
LLM and editorial assistance are permitted *after* your time budget expires and *only* if you then rewrite from scratch, unaided. The live code critique and mock interviews make undigested code obvious very quickly, so this is easily detected — but the real cost is yours.

**5. Type your code. Do not copy it.**
Including during live-coding in class. Muscle memory for the canonical templates is a real asset under interview pressure.

**6. Attendance at the Friday Arena is non-negotiable.**
Contests and mocks cannot be made up asynchronously — performing under observation is the entire point.

---

## 4. What to do in the first 30 minutes of Week 1

Post this as a pre-session checklist:

- [ ] Python 3.10+ installed and `python3 --version` runs
- [ ] VS Code installed with the Python extension
- [ ] LeetCode account created
- [ ] GitHub repo `dsa-<name>` created, folder structure in place, shared with instructor
- [ ] `progress-tracker.md` copied in
- [ ] Solved **LC 1 — Two Sum** *any way you can*, however slow or ugly, committed with the header block

That last item matters. It gives every student a baseline commit before instruction begins, and it gives you a free diagnostic of who can already do what — read all 14 before Session 1 and you will start the course knowing your cohort.

---

## 5. A note to set expectations

Tell students this directly in the first session:

> This course asks for about 24 hours a week. That is a lot, and it is deliberate. In 16 weeks you will solve around 320 problems — roughly what a self-directed candidate solves before interviewing, except you will have instruction, feedback, and nine timed contests attached.
>
> You will hit a wall around week 7, when we do recursion. Almost everyone does. It is not a signal that you cannot do this; it is the normal shape of learning recursion after two years of writing loops.
>
> The one thing that will sink you is quietly falling behind and not saying so. There are fourteen of you. I will notice — but it is far cheaper if you tell me first.
