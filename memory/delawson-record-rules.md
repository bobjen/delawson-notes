---
name: delawson-record-rules
description: Records vs extents vs appends: one size bound, whole records in an extent, and the block rule that removes seal splitting
metadata:
  type: project
---

**The rules that relate records, appends and extents** (all Bob, 2026-08-17;
`DESIGN.md` §8.2.3, §8.2.4, §8.1.1a-2, §6.2).

- **One bound for three things: 4 GB logical, 32 MB compressed on first write.** An
  append, a record and an extent all share it. The compressed half is **soft** — it
  bounds the first write, and later recompression may exceed it, so nothing may treat
  it as an invariant. The append half may yet come *down* because a message is not an
  extent (#53: a frame is capped at 1 MB).
- **An extent holds whole records; a record may span appends.**
- ⚠️ **The rule that makes it all work:** a record may fail to end in its block only
  if it began at the start of that block or in an earlier one — equivalently, **a
  block ends on a record boundary or holds no boundary at all.** So a block is
  indivisible and placeable, an extent seals *between* records, and the
  "committed some records but not all" answer describes a state that cannot arise.
  That retired a whole issue (#51) rather than building it.
- **Durability has two gates**: a quorum of instances, and a record that has
  finished. Bytes on three disks are not a record.
- **A terminator framing's data may not contain its terminator, and the client
  checks it** — the client is the last place holding a record as a record.

**Why the client frames the record:** it makes every client append end on a record
boundary, so a writer using `laws` cannot reach the awkward corners at all.

See [[delawson-next-append]] and [[delawson-two-spaces]].
