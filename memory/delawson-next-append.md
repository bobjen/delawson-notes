---
name: delawson-next-append
description: Append was built 2026-08-17 (three hops, EN vectors, 3-of-4); what is deliberately not built yet
metadata:
  type: project
---

**Append and read both work as of 2026-08-17.** `laws append /path --data "…"` creates
the object if it is not there, and `laws read /path --record 0` gives the record back.
See `DESIGN.md` §17.8 and §17.9 for what was built, §8.1.5-§8.1.6 and §8.2.3-§8.2.4 for
the design that arrived with it, and [[delawson-record-rules]] for the record rules.

The shape, so it is not re-derived: **FE → DN** for the object's stream, **FE → SN**
for the extent, **FE → primary EN** for the bytes, **primary → three secondaries** in
parallel, answered on three of four.

What matters and is easy to lose:

- **The SN is asked once per extent, not once per append.** FE remembers the open
  extent per `(DnId, StreamId)`; five successive appends produced zero SN requests.
  That is what keeps the SN out of the write path, and it is a cache, so an FE holding
  it still holds no state.
- **An EN learns of an extent from the first append that names it.** There is no
  create-extent on an EN and there does not need to be one.
- **The quorum counting is a type of its own**, apart from the reply, which is why it
  is unit-testable. Two failures settle it at once rather than after a timeout.

Deliberately not built, so nobody looks for it: **sealing** and the extent cap, the
sparse index, reading by key, scans, EN-internal staging, and the per-disk journal.
Sealing is next, by [[delawson-exercise-the-rare-path]].

⚠️ **Two placeholders to remember when quoting a read rate:** the EN parses an extent
from the beginning every time (no sparse index), and a read goes to the *primary* rather
than any instance — an unsealed extent differs between instances, because a quorum of
three plus local coalescing can leave interior holes.

⚠️ **The one known wrong thing:** the receipt's offset is a *compressed* position where
the design wants a logical one, because the append carries no logical length. It is
correct only while nothing compresses. Issue #48, and see [[delawson-two-spaces]].
