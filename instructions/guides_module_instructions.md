# Guides Module — Instructions

## Purpose
Store compiled guides — structured, standalone reference documents built from
chat-history Q/A exchanges, task executions, or a topic the user names. A guide
is a polished, readable document that aggregates the key information into a
single file.

> The guides module lives at the root level alongside other modules.
> There is ONE guides directory for the whole engine — `guides/`.

## Directory layout
```
guides/
├── INDEX.md          ← master index: tracks every guide file
└── *.md              ← individual guide files
```

## When to use the guides module

- Use the guides module when the user asks you to **build/make/create/compile/
  write/draft/produce/save a guide** — for ANY topic, named or not (e.g.
  "build me a guide for the FYP document set", "build the guide for the viva").
- The action + the word "guide" ALWAYS means: compile a guide file into the guides module.
- The guide is built from existing chat-history Q/A files (1st instance), from
  task execution outputs (2nd/3rd instance), or from the topic the user names in
  the prompt when no Q/A/T/E files are specified.
- If the user does NOT ask for a guide, do NOT create a guide file.

### The guide trigger — MANDATORY CHECK, strict rule

**FIRST ACTION IN EVERY PROMPT: scan the user's prompt for the guide trigger
BEFORE doing anything else — before the instance decision, before any file work.**

**The trigger is INTENT-based, not exact-phrase. The rule is:**

> If the prompt contains an ACTION WORD (build, make, create, compile, write,
> draft, produce, save, generate, prepare) **+ the word "guide"** → A GUIDE MUST
> BE BUILT. No exceptions. The guide trigger is NEVER optional and NEVER skipped,
> no matter what else the prompt contains.

| User says... | Guide must be built? |
|--------------|---------------------|
| "make the guide" | YES |
| "create a guide" | YES |
| "compile a guide" | YES |
| "save as a guide" | YES |
| "build me a guide" | YES |
| "build the guide for <topic>" | YES |
| "write a guide about <topic>" | YES |
| "build a guide on X" + extra requests in the same prompt | YES — the guide part must still be fulfilled |
| "build a guide" inside a task about something else | YES — do the task AND build the guide |
| any other action word + "guide" | YES |
| the word "guide" with NO action word (e.g. "see the guide", "where is the guide") | NO — that is a reference to an existing guide, not a build request |
| no action word + no "guide" | NO — follow the normal workflow for that instance |

**There is no ambiguity.** If the prompt contains an action word + "guide",
a guide file MUST be created. Not building the guide when the trigger fires
is a rule violation, not a stylistic choice.

## Naming conventions

### Guide files — guides/<filename>.md
- File name = source file numbers + "_" + descriptive name.
- Single-digit file numbers (1–9): join WITHOUT "_".
  - Q1 + A1 → `Q1A1_descriptive-name.md`
- File numbers with 2+ digits (10 and up): join WITH "_".
  - Q12, Q13, Q14 + A12, A13, A14 → `Q12_13_14_A12_13_14_descriptive-name.md`
- Topic-built guides (no source files): `<descriptive-name>-guide.md` in kebab-case.
- The descriptive name must say what the guide covers — never `guide.md`.

### Guide metadata block
Every guide file must start with:

```
---
source: Q1.txt / A1.md
date: YYYY-MM-DD
model: <model used>
agent: <agent used>
instance: 1
---
```

- `source`: the Q/A or T/E files the guide was compiled from.
  - If the guide was built from a named topic (no source files), write
    `source: prompt (topic: <topic>)` — the metadata block is still required; a
    missing source file NEVER blocks the guide build.
- `instance`: the instance number the source files belong to (1, 2, or 3).
  - Topic-built guides with no source files: use the instance the prompt resolved to.
- `date`: the date the guide was created.

## Workflow — guide trigger fired

### Step 0 — MANDATORY trigger check (never skipped)
Before the instance decision and before any other work, scan the prompt for an
action word + the word "guide".

- Trigger FIRED → a guide file MUST be created in `guides/` this turn. Do NOT
  answer the prompt without building the guide. If the prompt also contains
  other work, do that work AND still build the guide — the guide is never dropped.
- Trigger did NOT fire → follow the normal workflow for that instance.

### Steps (after the trigger fires)
1. Identify the source Q/A files (or T/E files) the user wants compiled into a guide.
   - If the user named a topic but no source files → build the guide from the
     topic itself, using the graphify graph to locate relevant files.
   - If the user named source files → use those files.
2. If source files were named, verify every source file exists. If any source file
   is missing → stop, report the error.
3. Read the source files (if any).
4. Compile the guide content — structured, standalone, readable (title, sections,
   code blocks, tables — whatever fits).
5. Write the guide file to `guides/<filename>.md`.
6. Update `guides/INDEX.md` with a row for the new guide — ALL fields filled
   (`index_entry_rules.md`).
7. Run `graphify update .`.
8. Tell the user where the guide file is.

### What the guide must contain
- A clear title.
- Metadata block at the top.
- Structured content: sections, headings, code blocks, tables, diagrams.
- Self-contained — a reader should understand it without reading the source files.

## INDEX.md maintenance — strict

### guides/INDEX.md
- One row per guide file.
- Columns: `| File | Source | Date | Model | Agent | Instance | Summary |`
- `Source`: the Q/A or T/E files compiled from (or `prompt (topic: …)`).
- `Date`: YYYY-MM-DD. `Model` / `Agent`: per `index_entry_rules.md`.
- `Instance`: 1, 2, or 3.
- `Summary`: ≤ 15 words.
- Append-only: never delete or edit existing rows.
- **FORCED (see `index_entry_rules.md`): the INDEX.md row is part of the guide
  build. A guide is NOT done until its row exists with ALL 7 fields filled.**

## Relationship to other modules
- Guides are compiled FROM chat-history Q/A files (or T/E outputs), but are
  stored in the guides module. Original files stay where they are — never moved.
- Guides are NOT notes (notes = topic-organized distilled knowledge in notes/).
- A guide may reference app code in `apps/` but never modifies it — app code
  changes are tasks in tasks-history, committed in the app's own repo.

## Error Handling
- If the guide trigger fires, a guide MUST be built this turn. Missing sources
  do NOT cancel the build — see the source rules above.
- If the user asks for a guide but specifies no topic and no sources → ask.
- If a source file referenced for a guide does not exist → stop, report the error.
- If `guides/INDEX.md` is missing → stop, report the error.
- If a guide file name collides with an existing file → use a new descriptive name.

## Cross-Module Protection
- Guides module is READ-ONLY visible to all other modules.
- Only the guides module writes into `guides/`.
- Other modules reference guides via their file paths but never write into guides/.
