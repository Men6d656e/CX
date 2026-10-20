# Instructions Registry

This directory holds the rule files. Each file governs exactly one module.
Rules live HERE — never inline in SYSTEM.md or INDEX.md.

| Module | File | Governs | Status |
|--------|------|---------|--------|
| chat-history | `chat_history_instructions.md` | `chat-history/` (1st instance) | Active |
| tasks-history | `tasks_history_instructions.md` | `tasks-history/` (2nd + 3rd instance) | Active |
| instance module | `instance_module_instructions.md` | the 3 input interfaces (chat/task/agent prompt) | Active |
| notes | `notes_module_instructions.md` | `notes/` (topic-organized notes) | Active |
| guides | `guides_module_instructions.md` | `guides/` (compiled guide documents) | Active |
| index entries | `index_entry_rules.md` | ALL INDEX.md files — forced field-filling rules | Active |
| graphify-out | `graphify_instructions.md` | `graphify-out/` (knowledge graph, incl. apps code) | Active |
| apps | `app_module_instructions.md` | `apps/` (codebases as git submodules) | Active |
| utility | `utility_module_instructions.md` | `utility/` (on-demand runnable procedures) | Active |
