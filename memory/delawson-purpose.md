---
name: delawson-purpose
description: DeLawson is meant to be open source, aimed at shared AI memory, and to beat incumbents on accessibility per dollar
metadata:
  type: project
---

**DeLawson is intended to be an open-source distributed data store** — the software itself,
runnable by anyone on their own hardware, not a service with a source drop attached.

**Why that matters, per Bob (2026-08-14):**

- **Shared AI memory** is a motivating use case: indexes maintained over shared experiences in
  real or near-real time. DeLawson suits it because append-at-the-end makes record numbers
  permanently stable and keyed objects give stable keys, so **external indexes never go stale**.
  Both properties were derived for other reasons and happen to be exactly what an index needs.
  Durability timestamps additionally make time-range queries work indefinitely at second
  granularity (`DESIGN.md` §9.7) — distinct from the minutes-long SCN snapshot horizon.
- **Open source lets storage live inside a compute cluster**, removing egress fees. Not
  marginal: for 1 PB read once a month, AWS egress is ~48× the cost of storing it, ~190× if
  read four times.
- **Standalone it should still win** — better accessibility per dollar than anything available,
  not merely cheaper cold storage.

**How to apply:** §12's price list still matters, but for a self-hosted cluster it is **internal
chargeback**, and the ADLS Gen1 failure recurs in internal form — teams pile into tiny streams
when metadata is free whether or not money changes hands (see
[[delawson-billing-must-track-cost]]). One tension remains open rather than resolved:
millisecond index lookups may want latency §1.4 deliberately deprioritizes, though
near-real-time is comfortable. Details in `DESIGN.md` §1.0.
