# Azure Data Lake Storage Gen1 — product overview

**Source:** <https://learn.microsoft.com/en-us/previous-versions/azure/data-lake-store/data-lake-store-overview>
Page dated 2019-04-17, archived (`is_archived: true`, `ROBOTS: NOINDEX,NOFOLLOW`).
Captured 2026-08-19. Pointed out by Bob as another **public-facing** documentation of
Cosmos, and flagged as *"retired and the documentation is likely to disappear entirely
sometime"* — which is why the substance is copied here rather than only linked.

⚠️ **It says very little that is solid** (Bob, 2026-08-19, and agreed). It is a product
overview, not an architecture document. What follows is everything technical in it.

## What it states

- **"Azure Data Lake Storage Gen1 is now retired… resources are no longer accessible."**
- **Unlimited storage, petabyte files.** *"It doesn't impose any limits on account sizes,
  file sizes, or the amount of data that can be stored… Individual files can range from
  kilobyte to petabytes in size."* And *"no limit on the duration of time for which the
  data can be stored."*
- **Striping for parallel read.** *"The data lake spreads parts of a file over a number of
  individual storage servers. This improves the read throughput when reading the file in
  parallel for performing data analytics."*
- **Durability by replication.** *"Data is stored durably by making multiple copies"* and
  *"by making redundant copies to guard against any unexpected failures."* No mention of
  erasure coding.
- **HDFS-compatible**, with a WebHDFS-compatible REST interface, and its own
  `adl://` filesystem said to allow *"further performance optimizations that aren't
  currently available in WebHDFS."*
- **Auth and access control.** Microsoft Entra ID (Azure AD) for identity, OAuth 2.0 in the
  REST interface, and *"POSIX-style permissions exposed by the WebHDFS protocol… ACLs on
  the root folder, on subfolders, and on individual files."*
- **Encryption is a choice at account creation** — *"You can choose to have your data
  encrypted or opt for no encryption."*
- **No schema on write**; data stored in its native format, interpretation left to the
  analytic framework.

## What it conspicuously does not state

No consistency guarantee. No durability figure. No throughput number. No block, chunk or
extent size. No quota or account limit of any kind. No description of how the striping is
decided or how many copies are made. **And no mention of compression**, which is the omission
that matters.

**Azure lists prices in GB, and ADLS Gen1 presumably billed the customer the same way** —
Bob's guess, 2026-08-19, from the general Azure convention rather than from that product's
price list. ⚠️ The unit is the weaker half of the argument anyway: a store billing compressed
bytes would also quote per GB, so "per GB" says nothing about *which* GB.

⚠️ **The silence is what carries it.** SCOPE says Cosmos stores data compressed, so a product
that compresses, charges by the GB and never mentions compression is almost certainly not
charging for the compressed byte — a provider billing compressed bytes has to explain
compression, because the customer cannot otherwise predict the bill. Recorded in `DESIGN.md`
§12.3.2 alongside BigQuery, which also defaults to logical.

## Why it is worth keeping

Three claims in it are useful as **public** evidence rather than assertion:

1. **Petabyte-scale single files were a shipped product claim**, not an aspiration — which
   is the scale DeLawson's own object-size work targets.
2. **Striping one file across many storage servers, justified by parallel read**, is the
   same argument DeLawson makes for spreading extents across ENs.
3. **A production store shipped encryption as opt-in-or-out**, where DeLawson has chosen
   always-on. Prior art for the weaker choice existing, not for it being right.

⚠️ It does **not** support the statement in `DESIGN.md` §12.1 that ADLS Gen1 did not bill
for user metadata. That remains Bob's own recollection, and this page is silent on pricing.
