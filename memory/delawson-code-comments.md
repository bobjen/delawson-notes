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

**A second rule, with a different trigger.** Bob, 2026-08-17, describing how he
reads code: *"If I find myself scanning a routine multiple times to figure
something out I'll put whatever it is in a comment above the routine, then my
scans can just look at the comment rather than rereading the code."* — labelling
routines with, for example, what locks they take.

⚠️ **This applies more strongly to an AI reader than to Bob, not less.** He
remembers "this one takes the shard lock" for months; a session does not, and
re-derives it from scratch every time or fails to. So the trigger to use is not
"I have scanned this three times" — it is **anything that had to be derived
rather than read, especially anything that caused a bug.** Bob has done almost no
reading of this code, so AIs are its main audience and these caches are the main
thing standing between a reader and a re-derivation.

The evidence: `describe` in the DN self-deadlocked `extend` by re-locking the same
shard and wedged every worker, and for a day afterwards it still carried no
comment saying it took a lock at all. State the hazard, not just the fact —
"takes the name's shard lock, so a caller holding one deadlocks" is checkable
where "uses locks" is not.

**Unusual memory sources go above the variable's declaration** (Bob, 2026-08-17).
In Rust the type says who owns a thing, so what is left worth saying is where the
*bytes* came from when the type cannot say: a slot in a segmented array reached by
id rather than reference, a slice into a serialized extent, a buffer off the
network, memory whose life is tied to a lock being held. Ordinary allocation needs
no comment; anything a reader would guess wrong about does.

**And say which space a number is in.** The read path had `record` meaning
object-local in one function and stream-local in another, which is the shape of the
numbering bug that survived until the first seal. Putting the space in the name —
`in_stream`, `in_extent` — is the same trick as caching a scan in a comment, applied
where a comment would be too far from the use.

**How to apply:** comment the *why* that the code cannot show — a trade-off
taken, a hazard avoided, a unit — once, at the definition. State the reasoning
without the citation: "a rule with too few domains is skipped, not failed" beats
"see §5.6.7". Module-level doc comments explaining what a file is for are
welcome; line-by-line narration is not. Pair with
[[delawson-name-length]], which puts the self-description in one comment at the
definition instead of in every use.
