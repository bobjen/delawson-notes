---
name: delawson-naming-style
description: DeLawson names stay plain and descriptive, not clever — so a wide range of people can work on the code
metadata:
  type: feedback
---

Keep identifiers **bland and descriptive**. No puns, no in-jokes, no
tongue-in-cheek names.

**Why:** Bob briefly floated the opposite — he enjoys bad puns, and cited a
colleague at Oracle who named the dictionary find-unique-key routine `dicfuk`
to get around a ban on swearwords — then reversed himself on 2026-08-14:
"blander names are a good idea to allow a variety of people to work with the
code." That reason is about who can join the project, which matters over a
multi-year horizon (see [[delawson-project-shape]]). The reversal is the
standing instruction; do not resurrect the pun latitude.

Concretely, he renamed the SecretStore ring from `SS` to **`KS` (Key Store)`**
after I pointed out the initials and that "key" is more accurate than "secret"
for what it holds.

**How to apply:** Machine-type convention in `DESIGN.md` §5.9 — the three
storage-hierarchy tiers end in `-N` (DN Directory Node, SN Stream Node, EN
Extent Node) and surrounding services do not (FE Front End, KS Key Store).
Follow that when adding a type. When naming anything else, prefer the obvious
descriptive word over the witty one.
