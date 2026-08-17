---
name: delawson-next-append
description: Append was built 2026-08-17 (three hops, EN vectors, 3-of-4); what is deliberately not built yet
metadata:
  type: project
---

**Append works as of 2026-08-17.** `laws append /path --data "…"` reaches an EN. See
`DESIGN.md` §17.8 for what was built and §8.1.5-§8.1.6 for the design added with it.

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

Deliberately not built, so nobody looks for it: implicit create (append to a missing
name is `not found`), sealing and the extent cap, record framing and record numbers,
EN-internal staging, the per-disk journal, and any way to read the bytes back.

⚠️ **The one known wrong thing:** the receipt's offset is a *compressed* position where
the design wants a logical one, because the append carries no logical length. It is
correct only while nothing compresses. Issue #48, and see [[delawson-two-spaces]].
