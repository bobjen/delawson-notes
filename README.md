# delawson-notes

Long-term memory and working notes for [DeLawson](https://github.com/bobjen/delawson),
a distributed store for very large objects.

The code lives in `~/src/delawson`; this repo holds the things that are not
derivable from the code — decisions and their reasons, working preferences,
and project state across sessions. Expect the project to run for years, well
past any single conversation's context.

## Layout

- `memory/` — one fact per file, with frontmatter (`name`, `description`,
  `metadata.type` of `user` | `feedback` | `project` | `reference`).
  Files cross-reference each other with `[[slug]]`.
- `memory/MEMORY.md` — the index. One line per memory. Loaded at the start of
  a session, so it stays short: pointers only, never content.

## Clean-room constraint

DeLawson is built from first principles. Nothing under `~/src/outlaws*` may be
read or used as a source — see `memory/delawson-clean-room.md`.
