---
name: delawson-next-implementation
description: The first implementation milestone — processes, config, a single-box test harness, and a delawson-to-FE0 ping used as a messaging-overhead benchmark
metadata:
  type: project
---

Design phase ended 2026-08-14. **Implementation starts 2026-08-15**, in this order (Bob's):

1. **Processes** — the machine-type daemons.
2. **Config files.**
3. **A test harness** that sets up processes and config files **on a single box**.
4. **A ping**: the `delawson` client to **FE0**, which answers `"FE0 is alive"`.
5. **A timing test of that ping.**

**The ping is a benchmark, not a feature.** Its actual work is under 1,000 instructions —
about 0.33 µs at 3 GHz — so essentially all of the measured time is **message overhead**.
That is the point: it establishes the baseline for building fast messaging.

For calibration, so tomorrow's first measurement has something to check against:

| Transport | Round trip | Share that is overhead |
|---|---|---|
| TCP loopback | ~40 µs | 99.2% |
| Unix domain socket | ~15 µs | 97.8% |
| io_uring / kernel bypass | ~8 µs | 95.8% |
| Shared memory, spinning | ~1 µs | 66.7% |

**Naming:** `FE0` follows the `DN0` convention from `DESIGN.md` §5.5.1 — numbered instances
per machine type.

**Environment as of 2026-08-14:** Rust 1.97.1, cargo 1.97.1, 16 cores, 30 GB RAM, one box.
`~/src/delawson` holds only `DESIGN.md`, `README.md`, `CLAUDE.md`, `.gitignore` — no
`Cargo.toml` yet, nothing scaffolded.

**How to apply:** start at step 1 and do not skip ahead to storage. The milestone is
deliberately about **process lifecycle, configuration, and message latency** — the things every
later section depends on and none of which the design document specifies. Expect to add design
sections as implementation forces decisions, and keep §0's labelling honest: measured numbers
replace guesses (see [[delawson-nullius-in-verba]], [[delawson-estimates-are-budgets]]).
