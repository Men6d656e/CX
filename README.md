# CX — Context Engine

Repo: https://github.com/Men6d656e/CX.git

A file-based context engine for **learning development**. Readable by any
agent, no database, nothing leaves the machine. This repo is NOT a copy of
any template — it is its own project.

## Members

| Member   |
|----------|
| Abdullah  |
| Akash    |
| Behzad   |

## Modules

| Module | Purpose |
|--------|---------|
| `chat-history/` | every user↔agent Q/A exchange, word-for-word (1st instance) |
| `tasks-history/` | task definitions, trackers, logs, executions (2nd + 3rd instance) |
| `instructions/` | the rule files — one per module, plus the instance decision flow |
| `notes/` | topic-organized knowledge distilled from consultations |
| `guides/` | compiled how-to guides (trigger: any action word + "guide") |
| `apps/` | the codebases — external repos added here as git submodules |
| `graphify-out/` | knowledge graph over this repo's context AND apps code |
| `utility/` | on-demand runnable procedures |

## apps/ — code lives here

- Code lives ONLY in `apps/`. Every codebase is its own git repo, added under
  `apps/<repo-name>` as a git submodule with its own remote and history.
- The context engine repo never holds application source directly — it holds
  the context (history, notes, guides, graph) around the code.
- The graphify graph indexes context files AND every submodule's code.

## Non-negotiable rules (enforced by the instructions)

- History is append-only: never edit or delete existing Q/A/T/E files.
- Every file created gets its index row in the SAME turn — all fields filled.
- Small Q/A and quick questions go through the instance flow (chat-history /
  tasks-history). Every prompt resolves to one of the 3 instances.
- Locate related files with `graphify query` FIRST — filesystem listing is
  the fallback. Run `graphify update .` after changes, from the repo root.
- All documents start as `.md`.

## Startup

Read `SYSTEM.md` first — it wires everything (INDEX, instructions, forced
rules). Then `INDEX.md` maps every module to its path and instruction file.
