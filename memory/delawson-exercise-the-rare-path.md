---
name: delawson-exercise-the-rare-path
description: Turn a configured limit small so the rare path becomes the common one on a single box
metadata:
  type: feedback
---

**Bob\'s way of testing a hierarchy on one box: shrink the limit until the rare
path is the common one.** Stated 2026-08-17 as the order for the write path
(`DESIGN.md` §17.9.4):

1. Append creates its object, and a read gets the record back. *(done)*
2. **Shrink the extent sealing limit** so sealing happens constantly: an extent
   seals, the SN allocates another, and a read finds records in both.
3. **Shrink the extents-per-stream limit** so a stream fills and the DN allocates
   another, driven by the previous stream reporting that it sealed.

**Why it matters beyond convenience:** each step turns a configured limit into a
constantly-exercised path rather than a new mechanism to write. A limit nobody
reaches is a code path nobody runs, and on one box the only way to reach it is to
move the limit. The small log files play the same trick.

The corollary for reviewing work: when something is only exercised at scale, ask
what knob would make it happen every second instead.

See [[delawson-test-strategy]] for the surrounding testing rules.
