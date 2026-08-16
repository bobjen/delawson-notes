---
name: delawson-issue-practice
description: How Bob wants GitHub Issues used on DeLawson — a cheap work queue, with #1 as the ship checklist and doc-update required before closing
metadata:
  type: feedback
---

GitHub Issues on `bobjen/delawson` are the work queue. **Filing is cheap and expected** — a large
project has millions. File whenever something should be fixed or is worth thinking about but acting
on it now would derail the current task. An issue only needs to state the question; it may leave
the solution open.

- **#1 "Ship DeLawson"** is the release checklist, with sub-issues linked from it.
- Grep open issues before filing; close genuine duplicates *as duplicates*, pointing at the
  canonical one.
- Order work by **(value delivered) / (remaining work)** — exactly when that is easy to compute,
  which it often is not.

**Why:** Bob's instruction, 2026-08-15. The load-bearing part is the asymmetry he named: **a closed
issue is essentially forgotten, whereas documentation is expected to be referenced for years.** So
part of closing an issue is making sure the documentation covers it.

**How to apply:** `DESIGN.md` §16 stays the canonical list of open design questions — do **not**
mirror it into issues wholesale, because that duplicates it into the medium with worse retention.
File an issue when a question blocks work or when work is ready to be done. Before closing anything,
update `DESIGN.md` first; an unreflected resolution means the issue is not done. The convention is
written into `~/src/delawson/CLAUDE.md` under "Issues" so it survives without this memory. See
[[delawson-project-shape]], [[delawson-nullius-in-verba]].
