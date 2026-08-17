---
name: delawson-next-append
description: Append is the next thing to build (session of 2026-08-17), and it is the first work that involves an EN
metadata:
  type: project
---

**Next session, 2026-08-17: append.** Bob's shape for it, stated 2026-08-16:

> Append will eventually be able to implicitly create an object if one does not exist
> (FE → DN → SN) plus create an extent and append to it (FE → EN). But create is just
> FE → DN → SN.

So append is the first operation that reaches an **EN**, and the first that needs
**extents**. What already exists to build on: `laws` → FE → DN → SN works for
`create`, `batch-create`, `object-info` and `batch-object-info`; DN indexes objects
by name and owns `StreamId`; SN keys streams by `(DnId, StreamId)` and holds a record
per stream with nothing in it yet.

What the design already decides, and should not be re-litigated while building:

- **An extent is named `(SnId, ExtentId, creation second)`** (§4.6.3), and the second
  is what selects the daily key — so an extent id needs the clock, and the SN must
  refuse to issue a second that goes backwards.
- **The write path is a broadcast to four instances with a 3-of-4 quorum** (§8.1.2),
  not a daisy chain, and **the SN is not in the write path** (§8.1.3).
- **Accepted, applied, durable are three different things** (§8.1.1), and the append
  returns a **receipt** (§8.1.1a-9, VSAM-like rather than POSIX-like).
- **The client compresses** (§8.1.4.1), which is as far upstream as compression goes.
- **EN-internal staging with common extents** (§8.1.4), and migration writes only the
  one or two instances the scheme needs (§8.1.4.4).
- **Nothing is persisted yet**, so an EN will hold bytes in RAM at first — the same
  stand-in bargain DN's hash table already is.

Two cautions from the object work that apply directly:

- **A create that loses a race must not leave a stream behind**; the same discipline
  will apply to an append that loses a race for an extent. Take the name, then make
  the thing.
- **The placeholder index degrades as it grows** (see [[delawson-measure-thrice]]),
  so quote an append rate against a stated size and from a fresh process.
