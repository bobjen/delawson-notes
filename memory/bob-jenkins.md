---
name: bob-jenkins
description: Bob Jenkins — 20 years on Microsoft Cosmos storage, 17 at Oracle on the database; author of ISAAC, lookup3, SpookyHash, jenny
metadata:
  type: user
---

Bob Jenkins. Resume: http://burtleburtle.net/bob/other/resume4.html (personal
site burtleburtle.net/bob, all personal projects public domain).
Email bob_jenkins@burtleburtle.net; GitHub `bobjen`.

CMU 1985–89: BS Computer Science plus a Masters in Mathematics (thesis on
HOMFLY polynomial algorithms); doctoral work in theoretical CS at UC Berkeley
1992–93, not completed.

**Microsoft, 2006–2026, Cosmos team** — distributed storage, ending
2026-07-01. Cosmos Store 2010–2026: in-memory B-trees, replication
(**20x throughput**), parallelized garbage collection, and **erasure coding**.
Earlier: Cosmos Store modelling and logging (CRC 6x faster), Cosmos PN, and
Scope 2007–2010 (SQL-like language with embedded C# expressions, parser).

**Oracle, 1989–2006** — the database kernel: numeric arithmetic (division 6x,
sqrt 66x faster), snapshots/materialized views, constraints (cascade deletes,
deferred constraints, skip lists), Unicode/character semantics for Oracle 9i,
function-based and descending indexes, the 100k-line recursive-descent SQL
parser, and resident expert on hash functions and collision analysis.

Published: **ISAAC** (PRNG, FSE 1996), **SpookyHash** (2012), lookup3,
**jenny** (pairwise test generator).

Languages on the resume: C, Java, C++, C#, PL/SQL, REXX. **Rust is not among
them**, though he chose it for DeLawson.

**How to apply:** He has two decades of production experience in precisely
DeLawson's problem domain — he has personally implemented erasure coding,
replication, and storage-layer B-trees at scale, and arrives with designs
already formed down to index granularities and device characteristics. Never
explain distributed-storage or database fundamentals to him, and do not offer
generic architecture menus; ask the one question that discriminates between
designs he might mean and expect a precise answer. Given the Rust gap, be
more explicit about Rust-specific idiom and API-shape tradeoffs than about
anything systems-level. See [[delawson-clean-room]] for the constraint his
Cosmos tenure imposes and [[delawson-project-shape]].
