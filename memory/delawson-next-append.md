---
name: delawson-next-append
description: The write and read path as of 2026-08-17 — append, read, both kinds of sealing, four FEs — and what is next
metadata:
  type: project
---

**As of 2026-08-17 the whole path works**: create, append (which creates the object if
needed), read by record number, extent sealing, stream sealing, and four front ends with
clients pinned to one each. `laws append /path --data "…"` creates
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

Deliberately not built, so nobody looks for it: the **sparse index**, reading by key,
scans, EN-internal staging, the per-disk journal, and hints as a user-visible thing
(#61).

**What is next, in the order the work suggests:** a benchmark for append and read —
every throughput number in `DESIGN.md` is still about pings and creates, and
`extents.seal_bytes` must come up from its deliberately tiny 4 KB before any of it
means anything. Then #59's open half (whether a segmented array can grow under the
shared lock), which gates the real metadata structures.

⚠️ **Two placeholders to remember when quoting a read rate:** the EN parses an extent
from the beginning every time (no sparse index), and a read of an *unsealed* extent goes
to the **primary**.

**Why the primary, stated correctly** (Bob corrected an earlier wrong reason of mine):
below the durable length every instance has everything, contiguously — interior holes
exist only *above* the watermark, on the primary too. The bytes do not differ. What a
secondary may have is a **stale durable length**, so it would answer against the wrong
watermark. Spreading unsealed reads is therefore a matter of getting a *number* to the
secondaries, not of moving data (#54).

⚠️ **The one known wrong thing:** the receipt's offset is a *compressed* position where
the design wants a logical one, because the append carries no logical length. It is
correct only while nothing compresses. Issue #48, and see [[delawson-two-spaces]].
