---
name: delawson-abstraction-cost
description: An interface is not free — package things behind one only where the boundary is wanted, and let Rust's module privacy do the rest
metadata:
  type: feedback
---

**Do not introduce an interface just because something could be packaged behind
one.** Bob, 2026-08-17: *"You can nicely package things internally behind small
interfaces, but then when you use them you have to use and update the interfaces,
and code is often easier to write and 2x or 3x more compact if you never introduce
an interface in the first place. Abstracting behind an interface can be good but
it's not always good."* In C++ his class members, for classes internal to the
project, are usually public.

**Why:** the interface is paid at every use and every change, and it also makes you
name the thing twice — the field and the accessor — which is a naming tax on top of
a code tax, and naming is the expensive part.

⚠️ **The Rust mapping is not obvious and it is favourable: the unit of privacy is the
module, not the type.** Every field of every struct in a module is reachable from
anywhere in that module, `pub` or not. So Bob's C++ habit is already the default
*within* a module, for free, with no accessors and no `pub` — and the interface tax
only arrives when something crosses a module or crate boundary.

**Which makes "should this be behind an interface" the same question as "should this
be a separate module"**, and therefore the same question as whether to split a file.
One act buys enforcement and costs compactness; do it where the boundary is wanted
and not otherwise.

**The codebase already does this without anyone deciding to**, measured 2026-08-17:
`pub` fields concentrate in `common/`, which crosses a crate boundary — 68 in
`config.rs`, 16 in `wire.rs` — while the per-service `main.rs` files have essentially
none, because their types are used only from their own module. And there is exactly
**one** `get_`/`set_` accessor in the whole tree.

**How to apply:** keep types that work together in one module and touch their fields
directly. Add `pub` when a boundary is genuinely crossed, and prefer moving the code
that needs the field into the module over exporting the field. Pairs with
[[delawson-name-length]] — a knot reaching into an edge's data is the normal case,
not a smell.
