---
name: delawson-software-should-not-matter
description: DeLawson's performance philosophy — measure distance from the hardware's theoretical bound, never distance from a competitor
metadata:
  type: feedback
---

**If the hardware can do it, the software should deliver it and not get in the way.** Message
overhead, disk throughput, CPU for compute — in each case the measurement is **distance from the
theoretical upper bound given the hardware**, not how much better than competitors.

**Why:** Bob's stated philosophy, 2026-08-15. "3× better than the alternative" is fully compatible
with running at 1% of what the machine can do, and the 1% is the interesting fact. Beating a
competitor is a moving target set by someone else's mistakes; the hardware bound is fixed and
computable.

**How to apply:**

- **Never report a throughput number without its computed ceiling and the fraction.** In `DESIGN.md`
  this is a labelling requirement alongside Derived/Cited/Guess/Asserted — see §1.6.1 and §0.
- **A large gap is a defect to file, not a fact to note.** The batched metadata path sits at ~5% of
  its bound and the unbatched at ~0.5%, and both already *meet* the shipping targets — the gap is
  tracked anyway.
- **The bound is for the work the design chose to do**, not the least work imaginable (§1.6.3).
  Otherwise the rule would forbid compression, whose 0.7%-of-memory-bandwidth cost is what the
  *algorithm* costs, not what the software wastes. Software may spend hardware to buy a stated
  benefit; it may not spend it for nothing.
- **Corollary that is easy to miss:** this obliges us to write down the **essential work per
  operation** first, because a fraction is meaningless without a defined denominator. Naming the
  essential work is what exposes the inessential.
- **Prefer fraction-of-bound targets to absolute ones** — they survive the decade-long media
  transition of §7.4, where "2M QPS" would age out.

Recorded in `DESIGN.md` §1.6 and its four subsections. See [[delawson-nullius-in-verba]], which is
the same discipline applied to design numbers rather than performance ones.
