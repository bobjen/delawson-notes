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

**Status, 2026-08-16 — steps 1-5 done, for two processes.** A cargo workspace with `common/`
(config, wire protocol, daemon lifecycle, placement report), `laws/` (the client, every command
present, unbuilt ones answering `unimplemented`), `ks/` (the template daemon), `config/`,
and `scripts/start-laws`, `stop-laws`, `bench-laws`. DN, SN, EN and FE are copies of `ks`, not
yet made. `DESIGN.md` §17 records the decisions; issues #2-#5 carry the detail.

**The ping went to KS0, not FE0, and that is deliberate:** Bob intends KS to be the **registrar** —
it listens on the one fixed port in a cluster and tells every other process what machine and port to
talk to (issue #36). So KS is what the others need before anything else works, which is why it was
built first. Its address cannot come from a registry, which is also what keeps cold start acyclic.

**The bar for ping is the prototype's ~1.4M QPS**, not §1.4's QPS targets: Bob corrected that on
2026-08-16 — the **200k QPS figure is ObjectInfo** (`laws` → FE → DN → SN, one of 20M metadata
records from RAM), four hops and a lookup, while the prototype's *ping* was ~1.4M. The first
implementation is 230k-400k, so it is **3-4x short of already-done work**, with nothing batched.

Measured on loopback TCP, retail, one round trip at a time per client: **`M` = 15-18 µs**, which is
the ~15 µs the table above calibrated for a *Unix-domain socket*, not the ~40 µs it guessed for TCP.
55.8k round trips/s with one client, 231k with 8, 400k with 16. With a reply body: **free to about
1 KB, per-byte past 8 KB**, ceilings ~730 MB/s for one client and ~2.4 GB/s for eight.

**KS is the registrar**, and the port assignment is split three ways: `start-laws` decides the port,
the process announces it to KS, KS answers `identity → (machine, port)`. A registration announces a
port and does not invent an identity — KS checks it against the roster. Ready means *findable*, not
merely listening. Built 2026-08-16.

**Batching means per-connection transport coalescing**, not a batch message (Bob, 2026-08-16): both
sides buffer frames and flush when nothing is left to read or answer. Worth **25x on one
connection** (59k → 1.48M round trips/s), which is where the prototype's ping bar sits; 8.4M at
16 connections x 256 in flight on this box, verified against KS's own answered-request counter and
costing 0.81 µs of server core per message. The 2M ObjectInfo figure was the *other* kind of
batching — an explicit request for 1,000 random objects.

**The dispatch finding is the one to remember:** answering ping on a worker instead of the read loop
caps throughput at ~200k however much load arrives, a 38x loss, because a scheduled handler flushes
its own answer and coalescing dies. The queue hop itself is only ~8 µs. So the question is never
"threads or async runtime" but "can answers still pile up before they are sent". Bob's guess for why
his tokio prototype ran 6x slower than this is the runtime, and this is the part of that guess this
box can test — supportive, not a verdict.

**Everything sent is encrypted and compressed in whole batches**, not per message: AEGIS-128X2 with
the nonce from a connection prefix, a direction byte and a segment counter. Encryption is free on
small messages and ~20% on large ones. **Headers compress even when bodies cannot** — a batch of bare
pings is 5.5x smaller with no user data in it, which is the argument for batch-level compression. A
failed attempt cost 39%, so a large batch is judged first by four sample pieces spread end to end
(a prefix is wrong: batches are heterogeneous).

**Logging, 2026-08-16.** Two rotating logs per process: the full one gets every line, the info one a
target share, with busy sites sampled at a stride a histogram keeps current. Line is
`n,date,pid=,tid=,file=,line=,message` with microsecond UTC; identity, host and version sit in a
header at the top of each file. **`notice` is the level above info** — the answer to "what is the
default for all but the chattiest lines". Three costs worth remembering: a filtered line 0.52 ns, a
once-a-second `logsec!` line 2.87 ns, and an actually-written line per message **85% of throughput**
(13.2M round trips/s to 1.9M), because a 105-byte line is five times the bytes a ping moves. **No
request id as a fixed field** — GC, sync and repair have no request.

**Objects, 2026-08-16.** create, batch-create, object-info and batch-object-info exist over
`laws` → FE → DN → SN. Every object gets its own stream, no inlining and no coalescing, because both
would hide what a create costs. **DN indexes objects by name and owns the StreamId; SN looks a stream
up by (DnId, StreamId)** and allocates neither. 20M objects created in 7.8 s (2.56M/s batched a
thousand at a time), 2.3 GB in DN at ~116 B each; random lookups 263k/s singly and 5.11M/s batched.
Both of §1.4's ObjectInfo bars cleared, but the comparison flatters us: no persistence, client shares
the box, and an object is a name plus a stream reference rather than a full metadata record.

**The three serializations that had to go**, each the same mistake in a different costume, and none
visible until the rate was high: a forwarding tier that **blocked a worker** while another process
worked (all-calls-asynchronous applies to *serving* — a handler now takes a `Reply` it carries into
the answer); **one syscall per peer request** under one lock (peer connections use the same batching
sender as answers); and **one connection between two processes being one read loop** on the far side
(a fan of connections per peer now).

**Logging is Bob's design, iterated hard.** Two rotating logs per process; a line is
`level,time,pid,tid,file,line,constant,values` with the values comma-separated and low-cardinality
first; one line per request written **when the answer is sent**, carrying the duration measured from
when the *batch* arrived. Kept at info despite costing 18-19% on single-object requests because it
settles live-site arguments ("SN is taking 50 seconds" vs "SN never got the message"). Files are
lz4-compressed at **7.5x**, in blocks sized from `min_write_bytes x compress_ratio` of the log disk's
media class, cut at line boundaries when a delayed write exceeds a block. Read by three answers -
histogram, grep, lines - served both by each process and by a standalone signed `logs` binary, serially
because it competes with the servers for CPU and disk. **The histogram has no knob**: a row per site,
and the row is what to paste into the grep.

**A batched ping was rejected** (2026-08-16): pipelining one client measures nothing the client
count does not. What replaced it is a **variable reply size** filled with random bytes — and the
generator is **Bob's own small fast one** (four words, three rotates, 64-bit rotates 7/13/37), since
he does not trust platform generators.

Two directives from that day that shape everything later: **every call in every service is
asynchronous** — request ids, any number outstanding, answers in any order — and **whether a
message is answered on the read loop or handed to a worker pool is per message type and a
measurement**, with ping almost certainly inline (issue #34).

**State at the end of 2026-08-16.** All five daemons exist (KS, DN, SN, EN, FE) plus `laws`, the
`common` library, a standalone `logs` reader, and the three scripts. KS is the registrar: it holds the
one fixed address, and everything else is handed a port by `start-laws`, announces it, and is found by
name. Every call is asynchronous, including *serving* — a handler takes a `Reply` it can carry into the
answer it waits for, so a forwarding tier costs a forward rather than a thread. Everything sent is
encrypted and compressed in whole batches. Objects can be created and looked up. Nothing is persisted,
nothing is deleted, there are no directories, no records, and no real keys.

The numbers that matter, all retail on one 16-core box with the client sharing it:

| | Rate | Spread |
|---|---|---|
| ping, 16 connections x 256 in flight | 12.8M round trips/s | 1.9% |
| ping, 1 connection x 64 | 1.48M | |
| object-info, one per request over 20M objects | 212k | 2.8% |
| batch-object-info, 1,000 per request | 5.1M lookups/s | |
| create, 1,000 per request, growing table | 2.8M objects/s | 51%, and that is the table |
| create, same with the table pre-sized | 3.79M objects/s | 3.4% |

**What is left open and worth reading before changing that code:** issues #37 (two-machine
measurement — every number above has the client on the same box), #39 (the send switch is one-way),
#41 and #42 (logging retention against line rate), #24 (message keys still a pre-shared test key), #44
(machine facts in the log tool), #35 and #36 (ring membership, and what registration should become).

**How to apply:** start at step 1 and do not skip ahead to storage. The milestone is
deliberately about **process lifecycle, configuration, and message latency** — the things every
later section depends on and none of which the design document specifies. Expect to add design
sections as implementation forces decisions, and keep §0's labelling honest: measured numbers
replace guesses (see [[delawson-nullius-in-verba]], [[delawson-estimates-are-budgets]]).
