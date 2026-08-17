---
name: delawson-test-strategy
description: Unit tests are the bulk, one e2e test runs every command once, benchmarks are occasional and discardable
metadata:
  type: feedback
---

**How DeLawson is tested** (Bob, 2026-08-17). `DESIGN.md` §15.3, and issue #45 tracks
the gap between it and the tests that existed.

The kinds differ in what they cost to *keep*, and that is what sets how many:

- **Unit tests are the bulk.** Single-threaded, call the code directly, depend on
  almost nothing, so they break only when their own code changes. The whole suite runs
  in under a minute **because it runs before every push**.
- **One end-to-end test, executing every command once.** An e2e test's real cost is not
  that it is slow — it is that everything which changes breaks it. One catches the
  failure a unit test structurally cannot (a command that does not run at all).
- **Benchmarks are occasional**, carry their latest numbers **in a comment beside the
  benchmark**, and are versioned when those numbers move ~5%. One that has finished the
  tuning it was written for is often better discarded than kept.

Two rules with teeth:

- ⚠️ **Touching the disk does not make a test end-to-end.** A log-writer test writes log
  files, because files are what that code does. What a unit test may not read from disk
  is a **configuration** — that is a dependency on the deployment rather than the code.
  (`common::testing`, behind a `testing` feature, builds them in code instead.)
- **The real code usually has to change** to be callable — logic reachable only through
  a request handler is logic whose shape has not been factored yet. Tests are real code,
  so they do not repeat themselves either.

See [[delawson-feature-parameters]] for why the test config has features on, and
[[delawson-measure-thrice]] for the three-trials rule benchmarks still obey.
