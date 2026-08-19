---
name: delawson-name-length
description: DeLawson identifiers stay at 20 characters or fewer, ideally 15; describe them once at the definition
metadata:
  type: feedback
---

**Every name in DeLawson code is 20 characters or fewer**, and worth a second
attempt at 15 if it still reads. Do not make identifiers fully
self-descriptive; put the self-description in **one comment at the
definition**.

**Why:** Bob, 2026-08-16: "I tend to automatically translate identifiers longer
than 20 characters as 'thunk', and `for (thunk thunk; thunk < thunk+thunk;
thunk++) thunk` loses too much information to be interpretable." The cost of a
long name is paid at every *use*, in the reader's working memory, while the
benefit could have been paid once at the definition.

**The ceiling is not flat — it is a gradient by what the code is doing** (Bob,
2026-08-17). He splits code into **knots** and **edges**:

| | Does | Names |
|---|---|---|
| **knot** | interesting combinations of variables | **1 to 5 characters**, to keep the logic compact |
| **edge** | a pipe information flows through without mixing | 15 to 20 is common |
| config parameter | — | sometimes beyond 20 |

**Knots are fed by edges**, so a knot's first act is to copy what it needs into
short-named locals. Which reconciles this with [[delawson-naming-style]]'s rule that
an index is named for what it indexes: **name length tracks how far the reader is
from the definition.** In a knot the definition is a line or two above and the whole
knot is read at once, so two characters carry fine. Along an edge the name is all the
reader gets, because they are skimming past.

**A quantity's name says what kind of quantity it is.** Bob writes `nXxx` for "number
of Xxx" alongside `iXxx` for "iterator over Xxx". The prefixes do not survive the move
to snake_case, but the project already keeps the principle in its own spelling: a
**plural noun is a count** (`records`, `instances`, `streams`) and an **index is named
for the thing it indexes** (`en`, `col`). Nothing to change — just worth writing down
so it does not drift.

## Hoist the loop bound into a local

Bob's example of code he would not accept, 2026-08-17:

```
int nW=0; for (int iOff=0; iOff<string.length(); iOff++) if (string[iOff]=='w') nW++;
```

The fault is that `string.length()` is not in a local: it gets evaluated every
iteration, *"which is both slow and I'm hoping it stays a single value."*

⚠️ **The second reason is the stronger one, and it is about correctness rather than
speed.** A bound re-read each time round can *be* a different value, and then the loop
is not the loop that was reasoned about. Copying once is what makes the computation see
one value throughout — and reading state at one moment while acting on it at another is
the shape of the missed wakeup this project has already paid for.

**Rust changes where this bites, and does not remove it.** `for off in 0..s.len()`
evaluates `len()` once when the range is built, so the C++ hazard is structural to the
three-part `for` and simply absent here. What survives:

- a `while` condition, which really does re-read every time;
- an **atomic**, which reloads on every read by definition, so `while i < n.load(..)`
  is the same bug in a new spelling;
- a field behind a lock guard or an `Arc`, which the compiler must reload across any
  call it cannot inline.

So the discipline belongs exactly where Bob puts it — shared or repeatedly-evaluated
state — and a plain local `Vec` needs no help.

**How to apply:** `repair_bytes_sec` not `repair_bandwidth_bytes_per_sec`;
`compress_read_win` not `compression_is_read_win`; `min_read_bytes` not
`minimum_read_bytes`. It applies to test function names too. This is the same
instinct as [[delawson-naming-style]] — plain, unclever names — pushed on
length, and it works with [[delawson-code-comments]]: fewer comments, but the
one at the definition carries the meaning.
