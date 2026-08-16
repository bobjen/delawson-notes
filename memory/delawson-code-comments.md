---
name: delawson-code-comments
description: Comment sparingly in DeLawson code, and never cite DESIGN.md section numbers from code
metadata:
  type: feedback
---

Be **sparing with comments** in DeLawson source. And **do not put `DESIGN.md`
section references in code** — no `(§7.4.1)`, no `per §5.6.7`.

**Why:** Bob, 2026-08-16, on the first Rust code written for the project:
"they'll go stale if you don't and it's not worth maintaining them. Those
section references for example will go stale immediately." `DESIGN.md` is
renumbered and rewritten constantly — §17 was appended the same day — so a
citation in code is a pointer that rots within days, and a comment that
restates the code is a second thing to keep true.

**How to apply:** comment the *why* that the code cannot show — a trade-off
taken, a hazard avoided, a unit — once, at the definition. State the reasoning
without the citation: "a rule with too few domains is skipped, not failed" beats
"see §5.6.7". Module-level doc comments explaining what a file is for are
welcome; line-by-line narration is not. Pair with
[[delawson-name-length]], which puts the self-description in one comment at the
definition instead of in every use.
