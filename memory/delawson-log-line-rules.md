---
name: delawson-log-line-rules
description: How to write a DeLawson log line — two messages, constants separate kinds, new lines start at notice
metadata:
  type: feedback
---

Writing a log line in DeLawson follows four rules, all Bob's, settled 2026-08-16:

1. **Two messages: a constant and the values.** `logi!("answered", "op {},status
   {},about {},us {}", …)`. The constant is what a histogram groups on, so grouping
   needs no guess about where the words stop.
2. **Constants separate kinds; values carry detail.** A failure logged with the same
   constant as a success is one row among a million and invisible — a rare thing must
   have its own constant, or the histogram's rare end cannot find it. This was learnt
   by trying to demonstrate the rare end and failing: two deliberate failures hidden
   among 270,000 successes did not show up.
3. **A constant may not contain a comma** (it is a field); say "because" instead. A
   debug assertion catches it. Separate values with commas too.
4. **A new line is `notice`, and only a histogram demotes it.** The aim is many sites
   in the code and few lines in the log, which works because most sites are rare. The
   ladder is a demotion path: `notice` is both logs, `info` is the full log plus a
   sample, `debug` is nowhere. Demote only with evidence — today exactly two sites are
   demoted, both measured.

**Why it matters beyond tidiness:** the per-request line exists to settle live-site
arguments — *"SN is taking 50 seconds"* against *"SN never received the message"* — and
it costs 18-19% on single-object requests, which Bob accepted for that reason. A line
that cannot be found or grouped is paying that price for nothing.

Ordering: the histogram sorts **rarest first, busiest last**, because it runs past a
screen and a terminal leaves the end in front of you. See [[delawson-code-comments]]
for the related rule about comments.
