# Memory index — DeLawson

One line per memory. Pointers only; content lives in the files.

- [Clean-room constraint](delawson-clean-room.md) — never read or reference `~/src/outlaws*`; DeLawson is built from first principles.
- [Bob Jenkins](bob-jenkins.md) — 20 years on Cosmos (erasure coding, replication, B-trees), 17 at Oracle; arrives with designs already formed.
- [Project shape](delawson-project-shape.md) — ~2-year build, Rust, two private repos, design-doc-first.
- [The name](delawson-the-name.md) — George DeLawson Lowry, who went by "Laws"; so "Laws" means DeLawson.
- [Naming style](delawson-naming-style.md) — plain descriptive names, no puns; machine types end in -N only inside the storage hierarchy.
- [Billing must track cost](delawson-billing-must-track-cost.md) — bill every scarce resource, not just bytes; under-billing metadata sank ADLS Gen1.
- [Estimates are budgets](delawson-estimates-are-budgets.md) — don't re-baseline round guesses like the ~100 B extent entry as fields accumulate.
