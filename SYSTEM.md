# SYSTEM.md — CX Context Engine

## Identity
This is the context engine for CX — a learning-development workspace shared by
three members: **Abdullah, Akash, Behzad**. The purpose is to learn software
development by building real codebases in `apps/` while keeping every
discussion, decision, and task recorded. Readable by any agent, no database,
nothing leaves the machine.

Repo: https://github.com/Men6d656e/CX.git

Core modules: `chat-history/`, `tasks-history/`, `notes/`, `guides/`,
`apps/`, `utility/`, `instructions/`.

## Startup — do this first in every session
1. Read `INDEX.md` — it maps every module to its path and instruction file.
2. Read the instruction file for the module your task touches.
3. Check the Forced Rules below — index entries and graphify-first apply to
   EVERY task, in EVERY instance, for EVERY agent.
4. For locating related files, use `graphify query` FIRST — filesystem listing
   is the fallback.

## Validation Checklist
- [ ] `INDEX.md` — wiring map of all modules
- [ ] `instructions/` — rule files directory + `instructions/INDEX.md`
- [ ] `graphify-out/graph.json` — knowledge graph exists (if missing: `graphify update .`)

If any of these are missing, stop and report — do not proceed with guessed paths.

## Forced Rules — never skippable

### Index entries — FORCED
- Every file a task creates gets its index row in the SAME turn, every field
  filled with real data (File, Type, Date, Model, Agent, Instance, Summary ≤ 15
  words). No empty cells, no `—`, no `TBD`, no "see file".
- One row PER FILE. A Q/A exchange gets 2 rows. A task creating 4 files gets 4 rows.
- A task is NOT done until all its files' rows exist.
- Full rules: `instructions/index_entry_rules.md`.

### Graphify-first file discovery
- Locate related files with `graphify query "<topic>"` BEFORE listing
  directories. Run `graphify update .` after changes, from the repo root only.

## Rules
- Never invent paths — take them from `INDEX.md`.
- Never write rules inside `INDEX.md` — rules live in `instructions/`.
- When the user says "see Q(N).txt and answer", follow
  `instructions/chat_history_instructions.md`.
- When the user asks to build/make/create/draft a guide (ANY action word +
  "guide"), follow `instructions/guides_module_instructions.md` — the trigger
  is mandatory.
- When the user says "use the utility <name>", follow
  `instructions/utility_module_instructions.md` exactly.
- History is append-only — never edit or delete existing Q/A or task files.
- Every prompt resolves to one of the 3 instances (Q file → 1st, T file → 2nd,
  neither → 3rd). See `instructions/instance_module_instructions.md`.
- Code lives only in `apps/`.
