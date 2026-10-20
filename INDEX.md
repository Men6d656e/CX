# <FYP-idea-NN> — Index

Wiring map only. Rules live in `instructions/`.

## Modules

| Module | Path | Instruction File | Purpose |
|--------|------|------------------|---------|
| chat-history | `chat-history/` | `instructions/chat_history_instructions.md` | Q/A exchanges, word-for-word (1st instance) |
| tasks-history | `tasks-history/` | `instructions/tasks_history_instructions.md` | Task definitions, executions, logs (2nd + 3rd instance) |
| instructions | `instructions/` | `instructions/INDEX.md` | Registry of all rule files |
| instance module | (concept) | `instructions/instance_module_instructions.md` | The 3 input interfaces: chat (1st), tasks (2nd), agent prompt (3rd) |
| notes | `notes/` | `instructions/notes_module_instructions.md` | Topic-organized notes distilled from consultations |
| guides | `guides/` | `instructions/guides_module_instructions.md` | Compiled guide documents (trigger: any action word + "guide") |
| contribution-history | `contribution-history/` | `instructions/contribution_history_instructions.md` | Append-only proof-of-work feed for the valuation tracker |
| deliverables | `deliverables/` | `instructions/deliverables_instructions.md` | Proposal, PPT, 4 codebase-guidance docs, final docs/PPT |
| app | `app/` | `instructions/app_module_instructions.md` | ONE codebase, ONE idea — the only place code is allowed |
| utility | `utility/` | `instructions/utility_module_instructions.md` | On-demand runnable procedures |
| graphify-out | `graphify-out/` | `instructions/graphify_instructions.md` | Knowledge graph over context AND app code |

## Flow

1. The user gives a task or references a file (e.g. "see Q1.txt and answer").
2. Resolve the input to one of the 3 instances (see instance module).
3. Read the instruction file for the relevant module.
4. Locate related files with `graphify query` first; filesystem listing is the fallback.
5. Follow that workflow exactly.
6. Real work items (proposal, PPT, 4 docs, code, docs, PPT) append a
   contribution file in the same turn — see `contribution-history/README.md`.
