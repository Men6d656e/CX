# Utility Module — Instructions

## Purpose
Store small, reusable, on-demand procedures ("utilities") — step-by-step files
that fix or sync a specific part of the engine when the user invokes them.
A utility is NOT a context module and NOT a rule file. It is a **runnable
procedure** the user calls by name when needed.

## Directory layout
```
utility/
├── INDEX.md           ← master index: metadata for every utility file
└── utilityfile(N).md  ← individual utility procedure files
```

## Naming conventions

### Utility files — utility/utilityfile(N).md
- Numbered sequence: `utilityfile1.md`, `utilityfile2.md`, …
- Never reuse a number. New utilities get the next free number.

### Utility metadata block
```
---
id: utilityfile(N)
name: <human-readable name of the procedure>
created: YYYY-MM-DD
model: <model that created it>
agent: <agent/app used>
instance: <instance that created it>
status: active | deprecated
---
```

### Utility INDEX.md
One row per utility file. Columns:
`| File | ID | Name | Created | Model | Agent | Instance | Status | Summary |`
- ALL fields filled per `index_entry_rules.md` — no empty cells.
- Append-only. Deprecated files keep their rows.

## When to use the utility module — the trigger

**The trigger is: the user says "use the utility <name>"** (or "run the
utility", "execute the utility"), where `<name>` is any of:

- the exact file name or id: `utilityfile1`
- the procedure name: `update-graphify` / "the utility that updates graphify"
- a close description: "the utility that syncs the graph"

| User says... | What happens |
|--------------|--------------|
| "use the utility update-graphify" | Run `utility/utilityfile1.md` exactly as written |
| "use the utility utilityfile1" | Same |
| "use the utility that syncs graphify" | Same — match by description |
| "use the utility <name>" where <name> matches nothing | STOP — report no match, list available utilities from `utility/INDEX.md`, create nothing |
| any prompt WITHOUT the utility trigger | never run a utility; follow the normal workflow for that instance |

### How to run a utility (mandatory)

1. **Trigger check:** scan the prompt for "use the utility ..." before other work.
2. **Resolve the file:** match `<name>` against `utility/INDEX.md` (id, name, or
   description). No match → stop and report.
3. **Verify the file exists** at `utility/<File>` (hard gate from
   `instance_module_instructions.md`). Missing → stop, report, create nothing.
4. **Read the utility file** and follow its Procedure section **in order, exactly**.
5. **Record:** if invoked inside an open task, record the run in that task's
   log and execution files. If invoked standalone (3rd instance), the utility
   run IS the task — normal 3rd-instance tracking applies.
6. **Report** the result in the terms the utility file specifies.

### Rules
- Utilities never replace the standing rules in `instructions/` — if a utility's
  steps conflict with `instance_module_instructions.md` or
  `index_entry_rules.md`, the instruction files win.
- Running a utility never bypasses the 3-instance decision.
- To change a procedure, create a new utility file with the next number and
  mark the old one `deprecated` (do not edit its index row).

## Relationship to other modules
- **graphify-out/**: utilityfile1 operates ON the graph (runs `graphify update .`
  from the repo root — never inside the app submodule). It must never hand-edit
  graph files.
- **chat-history / tasks-history**: a utility run inside a task is recorded in
  that task's log/execution files.
- **instructions/**: utilities are invoked BY rules but are not rules themselves.

## Adding a new utility
1. Create `utility/utilityfile(N+1).md` with metadata block + numbered Procedure.
2. Append its row to `utility/INDEX.md` — ALL fields filled.
3. Update nothing else — the module is wired once and stays wired.

## Error Handling
- Utility file named but missing → stop, report, create nothing.
- Utility name does not match anything → stop, report, list available utilities.
- Procedure fails mid-way → record the error (if in a task), report, do not improvise.
- `utility/INDEX.md` missing → stop and report.

## Cross-Module Protection
- Utility module is READ-ONLY visible to all other modules.
- Only the utility module writes into `utility/`.
