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

For calibration, so the first measurement has something to check against:

| Transport | Round trip | Share that is overhead |
|---|---|---|
| TCP loopback | ~40 µs | 99.2% |
| Unix domain socket | ~15 µs | 97.8% |
| io_uring / kernel bypass | ~8 µs | 95.8% |
| Shared memory, spinning | ~1 µs | 66.7% |

**Do not read that io_uring row as a plan.** Bob's prototype measured io_uring as a
**regression** on the network path: batching amortizes the same per-request syscall
overhead and did it better, and batching alone delivered all the gains. The point of
timing a single ping is to learn the per-message overhead `M`, which is what tells you
the **batch depth** needed to hide it. `DESIGN.md` §1.4.0.1 records this; io_uring for
**EN-to-SSD** is a separate, untried, positive-prior case.

**Naming:** `FE0` follows the `DN0` convention from `DESIGN.md` §5.5.1 — numbered instances
per machine type.

**Environment as of 2026-08-14:** Rust 1.97.1, cargo 1.97.1, 16 cores, 30 GB RAM, one box.
`~/src/delawson` holds only `DESIGN.md`, `README.md`, `CLAUDE.md`, `.gitignore` — no
`Cargo.toml` yet, nothing scaffolded.

**Still true as of 2026-08-15.** That day went to design instead — the append receipt, EN-internal
staging, media classes, EC group structure and evaluation, repair scheduling, metadata snapshots, and
placement degradation (`DESIGN.md` grew to ~6,500 lines, 95 open questions). **None of steps 1–5 has
been started.**

Two things from that day land directly on step 2, before any config file is written:

- **Config needs a media-class table** (§7.4.1). Index interval, minimum read size, compression block
  size, staging bypass threshold, and repair bandwidth are all per-device-class, not global. Putting
  them in one table now is cheap; scattering them as constants is expensive later.
- **The single-box harness has one domain at every level, so it satisfies no placement rule.** §5.6.7
  says an unenforceable rule is simply not enforced, so the harness is *legal* — but it must be
  visibly degenerate rather than silently reporting success, or the first thing the test
  infrastructure teaches the system is how to violate its own invariant.

**How to apply:** start at step 1 and do not skip ahead to storage. The milestone is
deliberately about **process lifecycle, configuration, and message latency** — the things every
later section depends on and none of which the design document specifies. Expect to add design
sections as implementation forces decisions, and keep §0's labelling honest: measured numbers
replace guesses (see [[delawson-nullius-in-verba]], [[delawson-estimates-are-budgets]]).
