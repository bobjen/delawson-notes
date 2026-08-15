---
name: delawson-billing-must-track-cost
description: DeLawson must bill for every scarce resource, not just bytes — under-billing metadata is what sank ADLS Gen1
metadata:
  type: project
---

Billing must be **proportional to actual cost, within 2× slack** (Bob's default), on
every scarce resource — bytes stored, but also stream count, extent count, and
object count. Bill too little and the service goes broke; bill too much and
competitors undercut it. Both bounds bind.

The 2× figure is a usable test, not a platitude. It is what forces **physical**
rather than logical byte billing, since compression ratios span 1× to 128× and no
single logical price sits within 2× of cost for everyone. It also forces metering
durability state per extent, since four replicas cost 2.67× a 6+3 encoding.

**Why:** Bob's account, 2026-08-14. Cosmos was made user-visible once as **ADLS
Gen1** (he notes the attempt is public knowledge). It did not bill for user
metadata, so users piled into enormous numbers of extremely tiny streams — the
cheapest way to use the product as priced — and that was a reason it failed. The
arithmetic is brutal: **DRAM costs ~5,000× per byte what disk does** on a full-TCO
basis — purchase price is the smallest term, server amortization dominates, and the
3-member ring triples it. So a minimal stream (~300 B of SN memory) costs what
1.5 MB of disk costs, and any stream holding under **~375 KB** is underbilled.
Filling one SN's stream budget with 1 KB streams is ~375× underbilled.

DN metadata sits on disk, so its break-even is ~112 B rather than ~375 KB — the
stream case is **~3,300× more dangerous**. That is the real reason tiny objects are
lumped into shared streams: it moves their metadata from the RAM tier to the disk
tier, not merely from many streams to few.

**How to apply:** Never simplify billing down to bytes, however tempting the "95% of
cost is disk" figure in `DESIGN.md` §1.2 makes it — that figure is an *outcome* of a
healthy workload, and billing only for bytes is what destroys the condition that
makes it true. When adding any mechanism that consumes a bounded resource, ask what
meters it. Details in §12; this memory exists because the lesson is easy to lose and
expensive to relearn. See [[delawson-estimates-are-budgets]].
