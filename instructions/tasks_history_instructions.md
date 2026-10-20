# Tasks-History Module — Instructions

## Purpose
Track every task from definition to completion: the task itself, the agent's
work, in-flight progress, and a log of what was done — so any agent can resume
work exactly where the last one stopped. This file is the ONLY set of rules for
the tasks-history module.

> This module is the **2nd instance** of the instance module, and is also where
> the **3rd instance** (agent's own prompt interface) auto-creates its tasks.
> See `instance_module_instructions.md` for the full picture.

## Directory layout
```
tasks-history/
├── INDEX.md         ← index of everything below (one row per file)
├── definitions/     ← T001.txt — task definitions (written by the human, or by the agent on 3rd instance)
├── executions/      ← E001.md  — agent work/output for the task (written by the agent)
├── active/          ← T001-tracker.md — in-flight progress while the task is open
├── logs/            ← T001-log.md — agent action log (what was done, in what order)
└── media/           ← images/screenshots the user drops or pastes for reference, tied to a T file
```## Naming conventions

- Task definition: `definitions/T(NNN).txt` — N is the next free integer,
  zero-padded to 3 digits: T001.txt, T002.txt, …, T010.txt, … Never reuse a number.
- Execution: `executions/E(NNN).md` — the number always matches the task:
  T003.txt → E003.md. E = Execution (agent/app output).
- Tracker: `active/T(NNN)-tracker.md` — created when work starts, updated as work progresses.
- Log: `logs/T(NNN)-log.md` — created when work starts; every step the agent
  takes is recorded here, in order.
- Media files: `media/<descriptive-name>.<ext>` — image or screenshot tied to a T file.
  Give it a descriptive filename that says what it is (e.g. `t002-error-screenshot.png`).
  Never rename or move a media file that an existing T file references.

## Workflow — "do task T001" (2nd instance)

### Pre-work file check — mandatory

Before doing anything else, verify that every file the user referenced actually
exists. This includes the T file itself and any media file it names.

- If `tasks-history/definitions/T001.txt` does not exist → stop. Reply with the
  error. Do not create a T file. Do not start the task.
- If the T file exists but names a media file that is not present in
  `tasks-history/media/` → stop. Reply with the error. Do not proceed without the
  referenced media file.
- Only after every referenced file is verified present does the agent continue.

### Steps after the check passes

1. Read `tasks-history/definitions/T001.txt` and any files it references.
2. Locate related past tasks: run `graphify query "<task topic>"` first (the
   graph indexes past T/E files AND app code); fall back to scanning
   `tasks-history/INDEX.md` only if graphify is unavailable or returns nothing.
3. Create `active/T001-tracker.md` (todo list) and `logs/T001-log.md` (action log) BEFORE starting.
4. Do the work. Track progress in the tracker; record actions in the log.
5. Write the result to `executions/E001.md`.
6. Append one row per new file to `tasks-history/INDEX.md`
   (`| File | Type | Date | Model | Agent | Instance | Summary |`, Instance = 2,
   ALL fields filled per `index_entry_rules.md`).
7. Run `graphify update .` from the repo root (app code changes in app/
   submodule are indexed too).
8. Tell the user where the execution file is.

## Workflow — 3rd instance (agent's own prompt interface)

### When this workflow is triggered (mandatory routing)

This workflow is triggered whenever the user sends a prompt directly to the agent
and that prompt does **not** name a Q file (1st instance) or a T file (2nd instance).
This includes:

- short prompts ("follow @SYSTEM.md and tell me the changes last night"),
- long prompts ("follow @SYSTEM.md and address the problem below: what is noir and what frameworks build zk apps"),
- prompts that only reference `@SYSTEM.md` (which is the bootstrap cue, not a task file),
- prompts that say "address the question/task/problem below".

Prompt length is **not** a signal. A short prompt is still a 3rd-instance task.
If the user wanted chat-history (1st instance), they would have named a Q file.

Do **not** answer such prompts inline. Do **not** treat them as casual chat.
Do **not** skip the tracking files because the prompt "looks small".

### Mandatory steps

1. Create `tasks-history/definitions/T(NNN).txt` containing the user's prompt
   word-for-word (next free number).
2. Create `active/T(NNN)-tracker.md` and `logs/T(NNN)-log.md`.
3. Locate related context: run `graphify query "<prompt topic>"` first; fall
   back to INDEX.md scan only if graphify is unavailable or returns nothing.
4. Do the work. Track progress in the tracker; record actions in the log.
5. Write the result to `executions/E(NNN).md`.
6. Append one row per new file to `tasks-history/INDEX.md` with **Instance = 3**
   — ALL fields filled per `index_entry_rules.md` — then run `graphify update .`
   from the repo root.
7. Tell the user where the execution file is.

### What must be in the auto-created T file

The T file the agent creates on the 3rd instance must contain:
- the user's prompt, word-for-word, at the top,
- a line saying which engine and which instance created it (Instance = 3),
- any media files the user attached or pasted, referenced by their filename in
  `tasks-history/media/`,
- nothing else invented by the agent. Do not reinterpret, summarize, or paraphrase
  the user's prompt before storing it. The T file is the record of what the user
  asked; it is not the agent's opinion of what the user meant.

### Media — user-placed, not agent-saved

Images for tasks are placed by the user into `tasks-history/media/`, before the
prompt is sent. The user gives the image a filename and references that filename
in the T file or in the 3rd-instance prompt. The agent reads the image from there.

This is the intended path. The agent does not normally save pasted images into
`tasks-history/media/` as the primary workflow. If the CLI hands the agent a
saveable pasted image and the user did **not** pre-place the image, that is a
fallback only: the agent may save it into `tasks-history/media/` so the task still
has its image, and then reference it by filename in the auto-created T file. Even
then, the image should end up in `tasks-history/media/` with a clear filename, not
left as a hidden attachment.

Do **not** store task images in chat-history/media. That directory belongs to the
1st instance. A task's image stays with the task.

### The 3rd instance with an image — the forced pattern

When the user wants to use the 3rd instance **and** include an image, the
compulsory pattern is:

1. The user places the image in `tasks-history/media/` and gives it a filename.
2. The user sends the 3rd-instance prompt. The prompt contains:
   - the system cue (`@SYSTEM.md` or equivalent),
   - the image filename,
   - the problem/task/question.
3. The agent reads the prompt, reads the named image from
   `tasks-history/media/`, creates `tasks-history/definitions/T(NNN).txt` that
   contains the prompt word-for-word **and** names the image file, then proceeds
   with the rest of the 3rd-instance workflow (tracker, log, execution,
   INDEX.md row, Instance = 3).

If the user sends a 3rd-instance prompt that references an image filename but that
image file does not exist in `tasks-history/media/`, the agent does **not**
create the T file, does **not** create any tracking files, and does **not**
proceed with the work. The agent replies only with the error: which file was
referenced, which directory it should be in, and that it is missing. Nothing is
written into the context engine.

### Media already inside a referenced T file (2nd instance)

When the user points at a T file and that T file references an image by name, read
the image from `tasks-history/media/` by that name. If the named file is missing,
stop — report it as an error. Do not guess a path, and do not fall back to
chat-history/media. Do not start the task without the referenced media file.

## Append-only rule
- Never edit or delete an existing task file. New work always gets new numbers.
- To extend a finished task, write a new definition that references the old
  number, e.g. "Extend T001 with …".

## Index maintenance
- Append one row per new file (definition, tracker, log, execution):
  ```
  | File | Type | Date | Model | Agent | Instance | Summary |
  |------|------|------|-------|-------|----------|---------|
  ```
- Instance: **2** for tasks defined by the human, **3** for tasks auto-created
  by the agent from the 3rd instance prompt.
- **FORCED (see `index_entry_rules.md`): one row PER FILE, every field filled —
  a task that creates 4 files gets 4 complete rows. A task is NOT done until
  every created file has its complete row. Empty cells, `—`, `TBD`, `(human)`,
  `Main` as an agent name are rule violations.**
- There is no Tokens column — token counts are not tracked.

## Status
- A task is done when `executions/E(NNN).md` exists and the user confirms.
- If blocked, write the blocker in `logs/T(NNN)-log.md` and tell the user — never silently skip.
- If a task spans multiple sessions, the tracker in `active/` is the handoff point for the next agent.

## Error Handling
- If `INDEX.md` is missing: cannot proceed — ask the user to verify the module exists
- If instruction file is missing: ask the user — rules cannot be guessed
- If path from INDEX.md doesn't exist: report the error — do not invent alternative paths
- If task definition file is missing: ask the user — do not create tasks from nothing
- If a T file references a media file that does not exist in `tasks-history/media/`: report it.
  Do not invent a path or pull a file from chat-history/media instead.

## Cross-repo protection
- **READ-ONLY access to sibling repos:** When reading another FYP-Desk repo (fyp-ideas, contribution-tracker-valueation, specilized-agents-skills-from-agency-agents), read files only. NEVER write inside another repo.
- **Write only to your own engine:** All writes (T/E/tracker/log files, indexes)
  happen in THIS engine's directories only.
