---
name: delawson-ec-patent
description: The ideal erasure code for DeLawson (maximally recoverable codes with locality) is patented until 2034 — Bob is a co-inventor, which makes it easier to walk into
metadata:
  type: project
---

The public paper is ["Explicit Maximally Recoverable Codes with
Locality"](https://arxiv.org/html/1307.4150v2) — Gopalan, Huang, **Jenkins**,
Yekhanin, 2013. Bob is a co-inventor.

**Why the co-inventorship is a hazard rather than a help:** the construction is
familiar to Bob and the paper is public, so it would be easy to reach for it
without noticing there is an IP question at all. Being an author confers no
licence.

**Decision:** the purpose of DeLawson is communication of designs as much as being a product,
which requires implementing EC properly. Rectangular LRCs are already public through the paper
and the patent, but DeLawson can show how to implement it in practice. So, fully implement 
rectangular LRCs, but protect them with a parameter and a warning on the parameter that it is patent protected.
Also implement turbocodes with similar functionality as the default, since they are not patent
protected, almost as space efficient, and allow DeLawson to be used in production without infringing the patent.
In particular, RS(8+2), with all symbols duplicated, uses the normal RS constants and requires 6 symbols to be
erased before suffering data loss.
