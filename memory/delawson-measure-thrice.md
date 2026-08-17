---
name: delawson-measure-thrice
description: Any timing in DeLawson is measured at least three times, and the spread is reported alongside the average
metadata:
  type: feedback
---

**Measure any timing at least three times and report the spread**, not just the
average.

**Why:** Bob, 2026-08-16: "Doing 3 measurements for any timing is a good thing, it
lets you see the variance as well as an average." Two wrong conclusions had already
been drawn from single samples that day — an "11% slower" that was noise, and a
"13% faster" that turned out to be a different variable (block size, not
compression). Bob spotted the second one himself, from the numbers not making
sense together.

**How to apply:** `laws bench`, `bench-create` and `bench-info` do three trials by
default and print each, then the mean and the spread. When quoting a number, quote
the spread with it. A spread that is large is itself the finding: create at a
thousand objects a request came out at 51%, which turned out to be the placeholder
hash table doubling under a lock — 1.8 second stalls — not noise at all. See
[[delawson-nullius-in-verba]].
