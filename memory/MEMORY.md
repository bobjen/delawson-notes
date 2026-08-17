# Memory index — DeLawson

One line per memory. Pointers only; content lives in the files.

- [Clean-room constraint](delawson-clean-room.md) — never read or reference `~/src/outlaws*`; DeLawson is built from first principles.
- [Bob Jenkins](bob-jenkins.md) — 20 years on Cosmos (erasure coding, replication, B-trees), 17 at Oracle; arrives with designs already formed.
- [Purpose](delawson-purpose.md) — open source, aimed at shared AI memory; co-location kills egress fees.
- [Append, as built](delawson-next-append.md) — three hops, EN vectors, 3-of-4; and what is deliberately missing.
- [Log line rules](delawson-log-line-rules.md) — two messages, constants separate kinds, new lines start at notice.
- [Next: implementation](delawson-next-implementation.md) — starts 2026-08-15: processes, config, single-box harness, delawson→FE0 ping as a messaging benchmark.
- [Project shape](delawson-project-shape.md) — ~2-year build, Rust, two private repos, design-doc-first.
- [The name](delawson-the-name.md) — George DeLawson Lowry, who went by "Laws"; so "Laws" means DeLawson.
- [Naming style](delawson-naming-style.md) — plain descriptive names, no puns; machine types end in -N only inside the storage hierarchy.
- [Billing must track cost](delawson-billing-must-track-cost.md) — bill every scarce resource, not just bytes; under-billing metadata sank ADLS Gen1.
- [EC patent](delawson-ec-patent.md) — maximally recoverable local codes are patented to 2034; Bob is a co-inventor, which makes it easy to walk into.
- [Nullius in verba](delawson-nullius-in-verba.md) — derive it or label it a guess; Bob's numbers carry no authority either.
- [Estimates are budgets](delawson-estimates-are-budgets.md) — don't re-baseline round guesses like the ~100 B extent entry as fields accumulate.
- [Public sources](delawson-public-sources.md) — Bob's five public pages: the AI memory essay, three scale pages, and the erasure-coding song.
- [Software should not matter](delawson-software-should-not-matter.md) — measure distance from the hardware bound, never from a competitor.
- [Comment sparingly](delawson-code-comments.md) — few comments, and never a `DESIGN.md` section number in code.
- [Name length](delawson-name-length.md) — 20 characters or fewer, ideally 15; describe once at the definition.
- [Measure three times](delawson-measure-thrice.md) — one sample is not a measurement; report the spread.
- [Issue practice](delawson-issue-practice.md) — Issues are a cheap work queue; #1 is the ship checklist; update the docs before closing anything.
- [Two spaces](delawson-two-spaces.md) — LOff and COff: logical and compressed are separate types so the compiler catches the mixup.
- [Feature parameters](delawson-feature-parameters.md) — checked in off, and the ring's leader reads it and sends it.
- [Test strategy](delawson-test-strategy.md) — unit tests are the bulk; one e2e test; benchmarks carry their numbers.
- [Record rules](delawson-record-rules.md) — one size bound for append, record and extent; a block ends on a boundary or holds none.
- [Exercise the rare path](delawson-exercise-the-rare-path.md) — shrink a limit until the rare case is the common one.
