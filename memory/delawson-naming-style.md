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

**An index is named for what it indexes** (Bob, 2026-08-17). He almost never writes
`i`: Gaussian elimination gets `iCol`, `iRow`, `iDiag`, *"so I can't accidentally
use the wrong one."* Reusing the same name in separate scopes of one file is fine —
scope disambiguates, and the point is not uniqueness but that a mix-up reads
wrongly.

⚠️ **The principle transfers, the spelling does not.** Rust is snake_case and this
project uses it, so the names are `col`, `row`, `diag`, `en`, `nth`. And Rust
removes most of the occasions: `for row in &rows` has no index to confuse in the
first place, so a bare index is already a signal that arithmetic is happening.

**Escalate to a type when the number travels.** A name protects one function; a
newtype protects every function it is passed through, which is why `LOff` and
`COff` exist for logical and compressed positions. The `space!` macro in
`common/src/offset.rs` is the tool. §14.6's structures are where this will bite
hardest — slot, id, segment number and index-within-segment are four confusable
integers in one place — so name the loop-local ones and give types to the ones that
cross a boundary. Pairs with [[delawson-two-spaces]].

**How to apply:** Machine-type convention in `DESIGN.md` §5.9 — the three
storage-hierarchy tiers end in `-N` (DN Directory Node, SN Stream Node, EN
Extent Node) and surrounding services do not (FE Front End, KS Key Store).
Follow that when adding a type. When naming anything else, prefer the obvious
descriptive word over the witty one.
