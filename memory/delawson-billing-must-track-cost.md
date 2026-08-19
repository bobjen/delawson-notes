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

**Two distinct meanings of "overhead"** — do not merge them, as I did once. The
95%-is-disk figure is about **hardware cost composition**: disks are 95% of what you
spend, everything else 5%. **Space efficiency** is separate and much bigger: only ~58%
of purchased disk bytes are billable, after not-yet-installed drives, down drives,
system metadata, an 80% fill target, and undeletable garbage. Wasted space comes out
of the 95%, not the 5%. `DESIGN.md` §12.7 has the chain, and it lands on a price of
~2.6x raw disk cost per byte sold.

**How to apply:** Never simplify billing down to bytes, however tempting the "95% of
cost is disk" figure in `DESIGN.md` §1.2 makes it — that figure is an *outcome* of a
healthy workload, and billing only for bytes is what destroys the condition that
makes it true. When adding any mechanism that consumes a bounded resource, ask what
meters it. Details in §12; this memory exists because the lesson is easy to lose and
expensive to relearn. See [[delawson-estimates-are-budgets]].

**The two bounds are not symmetric** (Bob, 2026-08-19): *"If DeLawson can store the same
data in less space and pass the savings on to customers, using less space makes DeLawson
more likely to get used. DeLawson billing just has to assure the cluster owner doesn't go
broke in the process."* The floor is the constraint; the ceiling is the lever.

⚠️ **So DeLawson bills physical bytes, and that is a competitive decision rather than an
accounting one.** Under logical billing the savings cannot reach the customer at all —
compression, coalescing and erasure coding would improve the owner's margin and nothing
else, and the customer would see the same bill a non-compressing competitor charges.
Physical billing also tracks cost by construction: `DESIGN.md` §12.3.3 measures a 13.5×
swing in revenue per physical byte under logical pricing, and billing the physical byte
makes that swing zero without anyone guessing a compression ratio.

**Which relocates the risk.** Bytes self-correct, so the ways an owner still goes broke are
metadata, operations and the transient double footprint during compaction — and not billing
metadata is precisely what ADLS Gen1 got wrong. Recorded in §12.1.0.

**And DeLawson ships a cost model, not a price list** (Bob, 2026-08-19). Billing is designed
so an operator can drive bills arbitrarily close to cost, because — with open source — that is
what makes the thing get used. An operator wanting more margin sets the coefficients: multiply
everything by X, or charge above cost on some dimensions and not others. *"It is always a
business in some sense, because actual clusters do cost money to run"* — which covers internal
chargeback as well as sales.

⚠️ **So the 2× slack is the error an operator cannot tune away.** Multiplying coefficients
changes the level, not the shape, so a dimension whose shape is wrong stays wrong at every
price. The design owes correct *dimensions*, not correct prices. Recorded in §12.0.
