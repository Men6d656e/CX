# Index Entry Rules — FORCED, applies to every INDEX.md

These rules apply to EVERY index file in this engine: `chat-history/INDEX.md`,
`tasks-history/INDEX.md`, `guides/INDEX.md`, `notes/INDEX.md` (root and
per-topic), `apps/INDEX.md`, and any future index.

> These are FORCED rules. "Forced" means: there is NO mode, no instance, no
> exception, no agent, no tool where these can be skipped. An agent that writes
> a file but skips its index row has NOT completed the task.

## Rule 1 — Every new file gets an index row (hard gate)

- Every file created in a module directory MUST get its index row in that
  module's INDEX.md in the SAME turn the file is created.
- The row is part of the task itself, not a follow-up. **A task is NOT done
  until every created file has its row.** Creating the file without the row is
  an incomplete task — the agent must finish it before reporting "done".
- Never defer the row to "later", to the user, or to a future session.

## Rule 2 — Every field must be filled (no empty cells)

Every column in every row must be filled with real data. Empty cells are
forbidden:

| Field | Must be filled with | Never acceptable |
|-------|--------------------|------------------|
| File | Exact file name/path of the new file | a placeholder, `TBD`, `—` |
| Type | The module's defined Type value | empty |
| Date | `YYYY-MM-DD` (creation date) | empty, `?` |
| Model | The model that produced the file (e.g. `big-pickle`, `glm-5.3-flash`, `openrouter/deepseek/...`). For human-written files: `Human` | empty, `N/A`, `—`, `(human)` |
| Agent | The agent/app used (e.g. `opencode`, `omp`, `buffy (freebuff)`, `kilo`, `hermes`). For human-written files: `Human` | empty, `N/A`, `Main` |
| Instance | 1, 2, or 3 per the instance module | empty, `0` |
| Summary | ≤ 15 words saying what the file contains | empty, `—`, "see file", "n/a", copying the title only |

- If you don't know the model/agent, you ARE the model/agent — fill in yourself.
- `Human` is the ONLY valid value for human-written files (Q files, T
  definitions written by the human).
- An empty cell, `—`, `TBD`, `(human)`, `Main`, or a title-copy "summary" is a
  rule violation, not a shortcut.

## Rule 3 — Multi-file tasks get one row per file

- A task that creates a T file + tracker + log + execution gets **4 rows**, one
  per file, each fully filled.
- A Q/A exchange gets **2 rows** — one for the Q file, one for the A file.
  NEVER combine a Q and its A into one row.
- Never batch files into one row. Never add only the "main" file's row.

## Rule 4 — Legacy file names (repair on next touch, do not rename)

Some early files use the old non-padded naming (`T1.txt`, `E1.md`) instead of
the zero-padded standard (`T001.txt`, `E003.md`). Rules:

- **Never rename existing files** — history is append-only.
- Their index rows stay as-is unless they violate Rule 2 (empty/placeholder
  fields) — if they do, repair the FIELDS (not the file names) on next touch.
- All NEW files use the zero-padded standard: `T002.txt`, `E002.md`, …

## Rule 5 — Registering a file whose index row was forgotten (repair)

If you find an existing file that has no index row (left by a previous agent):
- Repair it: append the row now, best-effort fields, Summary always real words.
- Run `graphify update .` afterwards so the graph picks up the changes.

## Rule 6 — Audit on every module write

- Before reporting a task done, verify: every file you created has a row, every
  field in your rows is non-empty and real.
- This audit is part of the workflow's final step — not optional self-check.
- If the module's INDEX.md is missing → stop and report (existing rule).
- If the index schema ever changes, update this file too.

## How this file is enforced

- SYSTEM.md lists these rules in its Forced Rules section — read at every session start.
- Every module instruction file's "Index maintenance" section links here and
  repeats the hard gate: **task is not done until all rows exist with all fields filled.**
