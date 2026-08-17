---
name: delawson-feature-parameters
description: A new feature is a parameter checked in off; one the ring must agree about is read by the leader and sent
metadata:
  type: feedback
---

**Every new feature is protected by a parameter, checked in disabled, enabled once
fully deployed** (Bob, 2026-08-17). `DESIGN.md` §13.2.5.

- **Ring members must behave the same.** A rollout is gradual by design, so if the
  *binary* decides whether a feature is on, the ring disagrees for the length of every
  deployment. A parameter keeps them agreeing.
- **A live site needs a switch that is not a rollback**, because rolling back binaries
  is the dominant data-loss mode in this design.
- ⚠️ **A parameter the ring must agree about is read by the leader and sent to the
  members**, never read by each member from its own file — configuration rolls out
  gradually too. Same rule for an extent's instances: the primary reads it and it
  travels with the append.
- **In tests the default is the opposite: on.** The checked-in cluster file and the
  checked-in test configuration deliberately disagree. A test wanting the old path
  turns it off itself.
- **The parameter and the old code it protects get deleted**, the next version or
  later. After a public release the upgrade graph sets that deadline: a feature cannot
  go while a supported upgrade path still runs a version needing it.

**Not needed yet**: nothing is persisted, so there is no old state to disagree with —
the first such parameter arrives with the first thing written to a disk that outlives
the process. So their absence today is correct.

See [[delawson-test-strategy]] for where the test-side default lives.
