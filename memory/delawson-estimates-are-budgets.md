---
name: delawson-estimates-are-budgets
description: Treat DeLawson's round-number estimates as budgets to spend against, not figures to re-derive every time a field is added
metadata:
  type: feedback
---

When a design figure is a round guess — the ~100 B extent metadata entry is the
live example — leave it alone as work proceeds. Do not re-baseline the
downstream arithmetic every time the itemized total changes.

**Why:** Bob's reason, 2026-08-14: "there will be flags and timestamps and maybe
other things as we go." An itemization that reaches 60 B today will drift back
toward 100 B as the design fills in, so re-deriving stream ceilings and
metadata sizes on each increment is churn that produces a number obsolete by
the next turn. The round figure is a budget with headroom, and the useful work
is spending it deliberately.

**How to apply:** State the current itemization once where it belongs, keep the
budget figure in the sizing analysis, and say explicitly that the itemization
is not a new baseline. Sensitivity tables should bracket the budget rather than
track the latest total. This generalizes past the extent entry — the same
applies to 256 GB machine RAM, 32 MB extents, 4/3 slack, and 20k fan-out.
Related: [[delawson-naming-style]] for the same preference against needless
polish.
