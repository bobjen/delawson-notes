---
name: delawson-clean-room
description: Never read or reference ~/src/outlaws* — DeLawson is a clean-room rebuild from first principles
metadata:
  type: feedback
---

Never read, grep, search, or reference anything under `~/src/outlaws*`, nor the
`github.com/bobjen/laws` and `github.com/bobjen/laws-notes` repos. Do not draw
designs, names, constants, or file layouts from them. Derive answers in
`~/src/delawson` or ask Bob.

**Enforced in the harness** as of 2026-08-14, at Bob's request: deny rules in
`~/.claude/settings.json` plus a `PreToolUse` hook
(`~/.claude/hooks/clean-room-guard.sh`, patterns in
`~/.claude/clean-room-blocklist.txt`) that denies any tool call whose input
matches. Writes under `~/src/delawson`, `~/src/delawson-notes`, and
`~/.claude` are exempt so the rule can be documented; `Bash` is never exempt,
so even a harmless command mentioning those names is refused.

**Why:** Bob worked on Microsoft Cosmos until 2026-07-01. The `outlaws*` tree
is a similar project written partly during that employment and is contaminated
with work-proprietary information. The whole point of DeLawson is to build the
same class of system from first principles, so its provenance has to stay
clean. Bob offered to delete `outlaws*` rather than have it looked at, which is
how strongly this is meant.

**How to apply:** Treat `outlaws*` as if it does not exist. If tempted to look
for a precedent there, that is exactly the moment not to. Everything in the
DeLawson repos must trace to Bob's statements, public sources, or reasoning
done in-session. See [[bob-jenkins]] and [[delawson-project-shape]].

Exposure to date: in the first session (2026-08-14) I listed the filenames
under `outlaws-notes/memory/` and saw 5 commit subject lines before being told
to stop; no file contents were read. `DESIGN.md` was written before that and is
uncontaminated.
