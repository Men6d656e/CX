# Chat-History Module — Instructions

## Purpose
Store every user ↔ agent exchange word-for-word, so nothing is ever lost and any
agent can pick up exactly where the last one left off. This file is the ONLY set
of rules for the chat-history module — read it whenever the user references a
Q file or asks to record an exchange.

> This module is the **1st instance** of the instance module. See
> `instance_module_instructions.md` for the full picture of the three input methods.

## Directory layout
```
chat-history/
├── INDEX.md       ← index of everything below (one row per file)
├── inputs/        ← Q(N).txt — user questions (written by the human)
├── outputs/       ← A(N).md  — agent answers (written by the agent)
└── media/         ← images/screenshots the user drops for reference
```

## Naming conventions

### Q files — inputs/Q(N).txt
- N is the next free integer: Q1.txt, Q2.txt, Q3.txt, … Never reuse a number.
- One file may hold several questions — number them 1., 2., 3. inside the file.
- Written by the human. If the user asks an agent to draft one, use the same naming.

### A files — outputs/A(N).md
- N always matches the Q file it answers: Q3.txt → A3.md.
- One A file answers EVERY question in its Q file, one numbered section per question.
- Start every A file with a metadata block:

  ```
  ---
  source: Q3.txt
  date: YYYY-MM-DD
  model: <model used>
  agent: <agent/app used>
  ---
  ```

### Media files — media/
- The human copies an image into `chat-history/media/` **before** sending the prompt,
  then references it by filename inside a Q file.
- The agent does not normally save pasted images into `chat-history/media/`.
  If the CLI hands the agent a saveable pasted image and the user did not pre-place
  it, that is only a fallback so the Q file still has its image.
- Never rename, move, or delete a media file that an existing Q file references.

## Append-only rule
- Never edit or delete an existing Q(N).txt or A(N).md.
- New exchanges always get NEW numbers. To redo or extend a topic, write a new Q
  file that references the old numbers, e.g. "Follow up on Q5 with more examples."

## Index maintenance
- After every completed exchange, append one row to `chat-history/INDEX.md`:

  ```
  | File | Type | Date | Model | Agent | Instance | Summary |
  |------|------|------|-------|-------|----------|---------|
  ```

  - File: exact file name (Q3.txt, A3.md, guide file, media file)
  - Type: Q | A | Guide | Media
  - Date: YYYY-MM-DD
  - Model: the model that produced it ("Human" for Q files)
  - Agent: the CLI/app used — opencode, cline, hermes, kilo, freebuff, … ("Human" for Q files)
  - Instance: always **1** (chat-history is the 1st instance)
  - Summary: ≤ 15 words

- **FORCED (see `index_entry_rules.md`): the index rows are part of the task.
  A Q/A exchange is NOT done until BOTH rows exist (one for the Q file, one for
  the A file) with ALL 7 fields filled. Empty cells, `—`, `TBD`, "see file"
  summaries are rule violations. Never combine a Q and its A into one row.**
- The index is the ONLY lookup point for history. Never skip updating it.
- There is no Tokens column — token counts are not tracked.

## Workflow — "see Q(N).txt and answer" (1st instance)

### Pre-work file check — mandatory

Before doing anything else, verify that every file the user referenced actually
exists. This includes the Q file itself and any media file it names.

- If `chat-history/inputs/Q1.txt` does not exist → stop. Reply with the error.
  Do not create a Q file. Do not answer from memory.
- If the Q file exists but names a media file that is not present in
  `chat-history/media/` → stop. Reply with the error. Do not proceed without the
  referenced media file.
- Only after every referenced file is verified present does the agent continue.

### Steps after the check passes

1. Read `chat-history/inputs/Q1.txt` and note every question.
2. Read any `media/` files referenced by name in the Q file.
3. Locate related context: run `graphify query "<Q topic>"` first (the graph
   indexes past Q/A files); fall back to scanning `chat-history/INDEX.md` only
   if graphify is unavailable or returns nothing.
4. Write `chat-history/outputs/A1.md`:
   - metadata block (source, date, model, agent)
   - one numbered section answering each question, in order
5. Append BOTH rows to `chat-history/INDEX.md` — one for the Q file, one for
   the A file, all fields filled (Instance = 1) — then run `graphify update .`.
6. Tell the user the answer file path (e.g. "answered in chat-history/outputs/A1.md").## Edge cases

- Q file missing or empty → stop and reply with the error. Do not guess or invent
  questions. Do not create a Q file unless the user explicitly asked the agent to
  draft one.
- User references another FYP-Desk repo (fyp-ideas, contribution-tracker-valueation, specilized-agents-skills-from-agency-agents) → read it read-only; never write inside another repo.
- User references another FYP-Desk repo (fyp-ideas, contribution-tracker-valueation, specilized-agents-skills-from-agency-agents) → read it read-only; never write inside another repo.
- User enters a task through the agent's own prompt interface → that is the 3rd
  instance, NOT chat-history. Route it to `tasks_history_instructions.md`.
- If the user references any other file by name ("see that file in abc repo",
  "use this file", etc.): verify it exists before proceeding. If it does not exist,
  stop and reply with the error only. Do not create anything in the context engine.

## Short prompts do NOT become chat-history automatically

A common mistake: a short prompt that references `@SYSTEM.md` is treated as a
quick question and answered inline or written to chat-history. That is wrong.

- If the prompt names a Q file → it is 1st instance (chat-history).
- If the prompt does not name a Q file → it is 3rd instance (tasks-history),
  even if it is short, even if it sounds like a question.

The agent decides the instance by whether a Q file is named, not by how long the
prompt is or how "question-like" it sounds.

## Error Handling
- If `INDEX.md` is missing: cannot proceed — ask the user to verify the module exists
- If instruction file is missing: ask the user — rules cannot be guessed
- If path from INDEX.md doesn't exist: report the error — do not invent alternative paths
- If another repo's files are referenced: read-only access only; never write to another repo's directories

## Cross-repo protection
- **READ-ONLY access to sibling repos:** When reading another FYP-Desk repo (fyp-ideas, contribution-tracker-valueation, specilized-agents-skills-from-agency-agents), read files only. NEVER write inside another repo.
- **Write only to your own engine:** All writes (Q/A files, task files, indexes)
  happen in THIS engine's directories only.
- **Reference, don't copy:** When another repo's info is needed, reference its files by path — do not duplicate or copy them.
- **Reference, don't copy:** When another repo's info is needed, reference its files by path — do not duplicate or copy them.