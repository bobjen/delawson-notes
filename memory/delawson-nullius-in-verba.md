---
name: delawson-nullius-in-verba
description: Every DeLawson design and number must be derived in place or labelled a guess — including Bob's own, which carry no authority
metadata:
  type: feedback
---

**Nullius in verba.** Take nobody's word for it, Bob's included. Any design or
number he supplies must either be **justified from first principles** in the
document where it appears, or **clearly labelled as a guess to confirm later**.

**Why:** his own instruction, 2026-08-14. He is the domain expert with twenty
years on Cosmos (see [[bob-jenkins]]), which makes deference the natural failure
mode — and an unlabelled number in a long design document is a liability, because
in a year nobody can tell whether it was computed or assumed. This has already
paid off in both directions: I caught 6+3 erasure coding failing the durability
target and derived that *p_gone* = 10^-4 implies an unnecessarily lax 58-hour
repair window, while he caught my RAM-to-disk ratio being 25x low.

**How to apply:** `DESIGN.md` §0 states the labelling convention — Derived,
Cited, Guess, Asserted, and ⚠️ for unresolved — and lists the inputs still
asserted rather than derived. Keep that list current. When Bob gives a figure,
the right response is to try to derive it, and to say so plainly if the
derivation disagrees; when it cannot be derived, label it and move on rather
than quietly adopting it. Related: [[delawson-estimates-are-budgets]], which is
about not *re-deriving* round numbers every time a field is added — the two
compose, since a budget is a labelled guess.
