# Memory index — DeLawson

One line per memory. Pointers only; content lives in the files.

- [Clean-room constraint](delawson-clean-room.md) — never read or reference `~/src/outlaws*`; DeLawson is built from first principles.
- [Bob Jenkins](bob-jenkins.md) — 20 years on Cosmos (erasure coding, replication, B-trees), 17 at Oracle; arrives with designs already formed.
- [Purpose](delawson-purpose.md) — open source, aimed at shared AI memory; co-location kills egress fees.
- [Next: implementation](delawson-next-implementation.md) — starts 2026-08-15: processes, config, single-box harness, delawson→FE0 ping as a messaging benchmark.
- [Project shape](delawson-project-shape.md) — ~2-year build, Rust, two private repos, design-doc-first.
- [The name](delawson-the-name.md) — George DeLawson Lowry, who went by "Laws"; so "Laws" means DeLawson.
- [Naming style](delawson-naming-style.md) — plain descriptive names, no puns; machine types end in -N only inside the storage hierarchy.
- [Billing must track cost](delawson-billing-must-track-cost.md) — bill every scarce resource, not just bytes; under-billing metadata sank ADLS Gen1.
- [EC patent](delawson-ec-patent.md) — maximally recoverable local codes are patented to 2034; Bob is a co-inventor, which makes it easy to walk into.
- [Nullius in verba](delawson-nullius-in-verba.md) — derive it or label it a guess; Bob's numbers carry no authority either.
- [Estimates are budgets](delawson-estimates-are-budgets.md) — don't re-baseline round guesses like the ~100 B extent entry as fields accumulate.
