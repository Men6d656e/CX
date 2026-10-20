# SYSTEM.md — <FYP-idea-NN> Context Engine

## Identity
This is the context engine for the FYP-idea project **<NAME>** (IDEA-NNN from
[fyp-ideas](https://github.com/FYP-DESK/fyp-ideas)). It manages all project work,
deliverables, decisions, progress, and the Q/A history behind every choice —
readable by any agent, no database, nothing leaves the machine.

Core modules: `chat-history/`, `tasks-history/`, `notes/`, `guides/`,
`contribution-history/`, `deliverables/`, `app/`, `utility/`, `instructions/`.

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

### Contribution-history — FORCED for real work
- Completing ANY real work item (proposal, PPT, 4 codebase-guidance docs,
  codebase development, final documentation, final PPT) REQUIRES appending one
  file to `contribution-history/` in the same turn. No contribution file = the
  work is not recorded = it cannot be valued or paid.
- Small questions/answers go through the instance flow only — never pollute
  contribution-history with them.
- Full rules: `instructions/contribution_history_instructions.md`.

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
- Code lives only in `app/`, and `app/` holds exactly ONE idea's codebase.
