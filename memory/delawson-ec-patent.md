---
name: delawson-ec-patent
description: The ideal erasure code for DeLawson (maximally recoverable codes with locality) is patented until 2034 — Bob is a co-inventor, which makes it easier to walk into
metadata:
  type: project
---

**Do not implement maximally recoverable codes with locality in DeLawson without
settling the patent question first.** A 2013 Microsoft patent covers the technique
and **does not expire until 2034**.

The public paper is ["Explicit Maximally Recoverable Codes with
Locality"](https://arxiv.org/html/1307.4150v2) — Gopalan, Huang, **Jenkins**,
Yekhanin, 2013. Bob is a co-inventor.

**Why it matters:** the technique is genuinely better on every axis than plain
Reed-Solomon — its Example 16 code, local (60,4,4), gives 1.33× overhead against
8+4's 1.50×, 4× repair amplification against 8×, and better durability. Locality
decouples repair traffic from *k*, so cheap storage and cheap repair stop being in
tension. That makes it exactly what DeLawson wants, and the temptation
correspondingly strong.

**Why the co-inventorship is a hazard rather than a help:** the construction is
familiar to Bob and the paper is public, so it would be easy to reach for it
without noticing there is an IP question at all. Being an author confers no
licence.

**How to apply:** the working assumption in `DESIGN.md` §4.3.2 is plain
Reed-Solomon with *m* = 4, with *k* chosen against the repair-traffic budget rather
than the storage ratio. Options before 2034 are licence, find an unencumbered
construction, or pay the repair traffic. This needs counsel, and it must be settled
*before* any code is chosen, because the choice is baked into every sealed extent
and those live for decades (§4.6.3). Also note 6+3 fails the 10^-14 durability
target — *m* = 4 is the floor. See [[delawson-clean-room]] for the related
provenance discipline.
