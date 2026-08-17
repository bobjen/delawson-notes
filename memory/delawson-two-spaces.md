---
name: delawson-two-spaces
description: Logical and compressed positions are separate Rust types (LOff, COff) so the compiler catches the confusion
metadata:
  type: feedback
---

**Logical and compressed byte positions get separate types, and the compiler is what
catches a mixup** (Bob, 2026-08-17). `common::offset::LOff` and `COff`, each a `u32`.

**Why it needs types rather than care:** both are 32-bit, an extent is capped well
inside that, and they are *equal* in exactly the case that hides the bug — while
nothing is compressing yet. So a test cannot find it and review will not either.

It paid for itself the hour it was written: typing the EN's offsets showed that the
append receipt returns a compressed position where the design requires a logical one,
because the append carries no logical length (issue #48). Nobody had noticed.

Two conventions that go with it:

- **A length in a space is the same type as a position in it**, deliberately. The
  confusion being prevented is logical against compressed, not position against length.
- **What `DESIGN.md` calls a *physical* position is what the code calls a *compressed*
  one.** Same space, two words, and §8.1.6 says so out loud.

See [[delawson-next-append]] for where they are used, and
[[delawson-software-should-not-matter]] for why a guess like this gets labelled.
