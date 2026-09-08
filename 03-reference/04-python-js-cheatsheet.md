# Python / JavaScript Cheatsheet

Python is the course's primary language; JavaScript is fully supported. **Students pick one and stay with it for at least the first eight weeks** — switching languages while also learning DSA means learning neither.

This file is the side-by-side reference. The **JS traps** section at the end is the one JavaScript students should read twice.

---

## Arrays / Lists

| Operation | Python | JavaScript |
|---|---|---|
| Length | `len(a)` | `a.length` |
| Index / last | `a[i]` / `a[-1]` | `a[i]` / `a[a.length-1]` or `a.at(-1)` |
| Slice | `a[1:4]` | `a.slice(1,4)` |
| Reverse (copy) | `a[::-1]` | `[...a].reverse()` |
| Reverse (in place) | `a.reverse()` | `a.reverse()` |
| Append / pop end | `a.append(x)` / `a.pop()` | `a.push(x)` / `a.pop()` |
| Prepend / pop front | `a.insert(0,x)` / `a.pop(0)` **O(n)** | `a.unshift(x)` / `a.shift()` **O(n)** |
| Sort in place | `a.sort()` | `a.sort((x,y)=>x-y)` ⚠ |
| Sort to a new list | `sorted(a)` | `[...a].sort((x,y)=>x-y)` |
| Sort by key | `a.sort(key=lambda p: p[1])` | `a.sort((x,y)=>x[1]-y[1])` |
| Sort by two keys | `a.sort(key=lambda p:(p[0],-p[1]))` | `a.sort((x,y)=>x[0]-y[0] \|\| y[1]-x[1])` |
| Fill | `[0]*n` | `new Array(n).fill(0)` |
| 2D grid | `[[0]*c for _ in range(r)]` ⚠ | `Array.from({length:r},()=>new Array(c).fill(0))` ⚠ |
| Concat | `a + b` | `a.concat(b)` or `[...a,...b]` |
| Contains | `x in a` **O(n)** | `a.includes(x)` **O(n)** |
| Index of | `a.index(x)` | `a.indexOf(x)` |
| Map / filter | `[f(x) for x in a]` | `a.map(f)` / `a.filter(f)` |
| Sum / max / min | `sum(a)`, `max(a)`, `min(a)` | `a.reduce((s,x)=>s+x,0)`, `Math.max(...a)` ⚠ |
| Enumerate | `for i, v in enumerate(a):` | `for (const [i,v] of a.entries())` |
| Zip | `for x, y in zip(a,b):` | `a.map((x,i)=>[x,b[i]])` |

---

## Hash maps

| Operation | Python `dict` | JavaScript `Map` |
|---|---|---|
| Create | `d = {}` | `const d = new Map()` |
| Set | `d[k] = v` | `d.set(k, v)` |
| Get | `d[k]` (raises) | `d.get(k)` (undefined) |
| Get with default | `d.get(k, 0)` | `d.get(k) ?? 0` |
| Contains | `k in d` | `d.has(k)` |
| Delete | `del d[k]` | `d.delete(k)` |
| Size | `len(d)` | `d.size` |
| Iterate | `for k, v in d.items():` | `for (const [k,v] of d)` |
| Keys / values | `d.keys()` / `d.values()` | `d.keys()` / `d.values()` |
| Increment | `d[k] = d.get(k,0)+1` | `d.set(k,(d.get(k)??0)+1)` |
| Counter | `Counter(s)` | build manually with a `Map` |
| Default dict | `defaultdict(list)` | `if(!d.has(k)) d.set(k,[])` |

**Use `Map`, not `{}`, in JavaScript.** A plain object coerces keys to strings, so `obj[1]` and `obj["1"]` are the same entry — a silent and very common bug.

## Sets

| Operation | Python | JavaScript |
|---|---|---|
| Create | `s = set()` / `{1,2,3}` | `new Set()` / `new Set([1,2,3])` |
| Add / remove | `s.add(x)` / `s.discard(x)` | `s.add(x)` / `s.delete(x)` |
| Contains | `x in s` **O(1)** | `s.has(x)` **O(1)** |
| Size | `len(s)` | `s.size` |
| From list | `set(a)` | `new Set(a)` |
| To list | `list(s)` | `[...s]` |
| Union / intersection | `a \| b` / `a & b` | `new Set([...a,...b])` / `[...a].filter(x=>b.has(x))` |

---

## Strings

| Operation | Python | JavaScript |
|---|---|---|
| Length | `len(s)` | `s.length` |
| Char at | `s[i]` | `s[i]` or `s.charAt(i)` |
| Slice | `s[1:4]` | `s.slice(1,4)` |
| Split / join | `s.split(",")` / `",".join(a)` | `s.split(",")` / `a.join(",")` |
| To chars | `list(s)` | `[...s]` or `s.split("")` |
| Case | `s.upper()` / `s.lower()` | `s.toUpperCase()` / `s.toLowerCase()` |
| Strip | `s.strip()` | `s.trim()` |
| Replace | `s.replace(a,b)` (all) | `s.replaceAll(a,b)` ⚠ |
| Starts / ends | `s.startswith(p)` | `s.startsWith(p)` |
| Char ↔ code | `ord(c)` / `chr(n)` | `c.charCodeAt(0)` / `String.fromCharCode(n)` |
| Is digit / alpha | `c.isdigit()` / `c.isalpha()` | `/\d/.test(c)` / `/[a-z]/i.test(c)` |
| Build efficiently | `"".join(parts)` | `parts.join("")` |

**Strings are immutable in both.** Building with `+=` in a loop is O(n²) in Python; JavaScript engines often optimise it, but `join` remains the right habit.

---

## Deques, heaps, and other structures

| | Python | JavaScript |
|---|---|---|
| Deque | `from collections import deque` | **no built-in** — use an array with a head index, or write one |
| Push / pop front | `dq.appendleft(x)` / `dq.popleft()` **O(1)** | `arr.shift()` is **O(n)** — do not use it in a loop |
| Heap | `import heapq` (min-heap) | **no built-in — you must implement one** |
| Max-heap | push `-x` | your comparator |
| Sorted container | `bisect.insort` | none |
| Binary search | `bisect_left` / `bisect_right` | write it |

> **JavaScript students: you need a heap by Week 10 and a deque by Week 6.** Write both in Week 5 and keep them in a `utils.js` you import all term. This is the single biggest practical difference between the two languages for this course.

---

## Numbers

| Operation | Python | JavaScript |
|---|---|---|
| Integer division | `a // b` (floors) | `Math.floor(a/b)` ⚠ |
| Modulo (negatives) | `-7 % 3 == 2` | `-7 % 3 === -1` ⚠ **different** |
| Power | `a ** b` | `a ** b` |
| Infinity | `float('inf')` | `Infinity` |
| Max int | unbounded | `Number.MAX_SAFE_INTEGER` (2⁵³−1) |
| Ceiling division | `(a + b - 1) // b` | `Math.ceil(a/b)` |
| Abs / min / max | `abs`, `min`, `max` | `Math.abs`, `Math.min`, `Math.max` |
| Random int | `random.randint(a,b)` | `Math.floor(Math.random()*(b-a+1))+a` |

---

## Classes

```python
class Node:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

    def describe(self):
        return f"Node({self.val})"
```

```javascript
class Node {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
    describe() { return `Node(${this.val})`; }
}
```

---

## The eight JavaScript traps

Every JS student in this course hits at least three of these. Read them now, not at 11pm.

1. **`arr.sort()` sorts as strings.** `[10, 9, 1].sort()` gives `[1, 10, 9]`. Always pass a comparator: `arr.sort((a,b) => a-b)`.
2. **`{}` coerces keys to strings.** `obj[1]` and `obj["1"]` are the same key. Use `Map`.
3. **No heap and no deque in the standard library.** Write both and keep them.
4. **`arr.shift()` and `arr.unshift()` are O(n).** Using `shift()` as a BFS dequeue silently turns O(V+E) into O(V²).
5. **`/` never does integer division.** `7/2` is `3.5`. Use `Math.floor` — and remember it floors toward negative infinity, unlike truncation.
6. **`%` on negatives differs from Python.** `-7 % 3` is `-1` in JS, `2` in Python. For a non-negative remainder: `((a % n) + n) % n`.
7. **Bitwise operators coerce to 32-bit signed integers.** Convenient for bit problems, dangerous above 2³¹.
8. **`Math.max(...arr)` blows the stack on very large arrays.** Use `reduce` for arrays above roughly 100,000 elements.

## Two Python traps worth the same attention

1. **`[[0]*c]*r` aliases every row** to the same list. Use `[[0]*c for _ in range(r)]`.
2. **Mutable default arguments** (`def f(x, memo={})`) are shared across every call. Use `memo=None` and initialise inside — this bites specifically during the Week 7 memoization session.
