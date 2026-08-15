---
name: delawson-project-shape
description: DeLawson's scope, timeline, repos, and naming — a ~2-year first-principles build
metadata:
  type: project
---

**DeLawson** is a distributed store for very large objects (up to 2^64 records
per object, spanning many machines because no machine can hold one). Named
after Bob's great-grandfather, **George DeLawson Lowry**.

- Language: **Rust**.
- Started 2026-08-14. Bob expects it to take about **two years**, i.e. into
  mid-2028. Plan for continuity across many sessions, not for a sprint.
- Code: `~/src/delawson` → `github.com/bobjen/delawson` (**private**).
- Notes/memory: `~/src/delawson-notes` → `github.com/bobjen/delawson-notes`
  (**private**).
- Working style: Bob asked for a **design document first, no code**. That phase ended
  2026-08-14; implementation begins 2026-08-15 — see [[delawson-next-implementation]].

Note `~/src` is itself a git repo with no commits; both DeLawson repos are
independent repos nested inside it.

**Why:** None of this is recoverable from the code, and at a two-year horizon
the reasons for decisions outlive the conversations that produced them.

**How to apply:** Record durable decisions here as they are made rather than
assuming context survives. Architecture itself belongs in
`~/src/delawson/DESIGN.md`, not here — keep this repo for what the code cannot
say. See [[bob-jenkins]], [[delawson-clean-room]].
