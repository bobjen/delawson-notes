---
name: delawson-public-sources
description: Bob's five public writings that set DeLawson's motivation and scale ambition — the AI memory essay, three scale pages, and the erasure-coding song
metadata:
  type: reference
---

Five public pages by Bob supply DeLawson's motivation and its scale target. All are safe to cite
(public, his own), and `DESIGN.md` labels them **Cited** where used.

| Source | Cited in | What it supplies |
|---|---|---|
| [How I expect AI to eventually work](https://burtleburtle.net/bob/future/ai.html) | §1.0, §1.0.3 | the shared-AI-memory case, quantified |
| [Really big data centers](https://burtleburtle.net/bob/future/bigdata.html) | §1.0.4 | the scale ambition |
| [Dense torus core](https://burtleburtle.net/bob/future/dense.html) | §1.0.4 | the dense core in detail |
| [Torus Dyson swarm](https://burtleburtle.net/bob/scifi/dyson.html) | §1.0.5 | the **intermediate** scale — and neighbour preservation |
| [Erasure Coding (song, YouTube)](https://youtu.be/oak6U2yBy94) | §4.3.4.2 | independent clouds; mechanisms that resist the operator |

**Why these matter beyond being nice links:** the AI page is specific enough to convert §1.0's
motivation into requirements — thousands of minds, 1–100 TB each, and a *remember-test* cycle at
~10,000/s on SSD. That yields **~10^7 metadata QPS** and, more importantly, identifies the **only
serial workload in the design**: a thinking cycle is dependent step after dependent step, so
per-mind speed is capped by round-trip latency (~100 µs), and parallelism has to come from the
*number* of minds rather than from within one. Everything else in DeLawson is a §1.4
throughput problem; this one is not.

**The Dyson-swarm page is the one with a present-day consequence**, and not because of its scale. Its
property is that all satellites share one orbital period, so **neighbours stay next to neighbours** —
Starlink is a very flattened instance of this, with northward and southward paths a few km apart. A
neighbour-preserving topology is what lets §5.6.7's "decide placement once and never revisit it" survive
a substrate that is physically moving; in a naive swarm every neighbour relation churns and the
failure-domain hierarchy never settles. It is also the only one of the three scale pages whose element
count (2^18) sits *inside* DeLawson's 2^32 addressing.

The two data-center pages are the scale ambition, and Bob's own caveat is that it is "utterly
wishful thinking that this particular code will go that far" — the dense core wants ~10^57 bits
where DeLawson addresses ~10^26. Worth keeping the gap quantified rather than vague: it is ~10^31,
so the target is **directional**. Two parts of the design do come from it, though — §1.3's refusal
of absolute epochs, and the 2^64 records per object.

**How to apply:** cite rather than paraphrase, and do not treat the scale pages as requirements.
When a design question touches *why* DeLawson exists, the AI page is the primary source and is more
specific than the summary in §1.0. See [[delawson-purpose]], [[delawson-software-should-not-matter]].
