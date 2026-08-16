---
name: delawson-name-length
description: DeLawson identifiers stay at 20 characters or fewer, ideally 15; describe them once at the definition
metadata:
  type: feedback
---

**Every name in DeLawson code is 20 characters or fewer**, and worth a second
attempt at 15 if it still reads. Do not make identifiers fully
self-descriptive; put the self-description in **one comment at the
definition**.

**Why:** Bob, 2026-08-16: "I tend to automatically translate identifiers longer
than 20 characters as 'thunk', and `for (thunk thunk; thunk < thunk+thunk;
thunk++) thunk` loses too much information to be interpretable." The cost of a
long name is paid at every *use*, in the reader's working memory, while the
benefit could have been paid once at the definition.

**How to apply:** `repair_bytes_sec` not `repair_bandwidth_bytes_per_sec`;
`compress_read_win` not `compression_is_read_win`; `min_read_bytes` not
`minimum_read_bytes`. It applies to test function names too. This is the same
instinct as [[delawson-naming-style]] — plain, unclever names — pushed on
length, and it works with [[delawson-code-comments]]: fewer comments, but the
one at the definition carries the meaning.
