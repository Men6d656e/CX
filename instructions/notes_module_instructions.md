# Notes Module — Instructions

## Purpose
Store important, distilled notes from consultations, organized by topic directories.
This is NOT a copy of chat-history — notes are curated, topic-organized summaries.

## Directory layout
```
notes/
├── INDEX.md          ← master index: tracks all topic directories and their contents
└── topics/           ← one directory per topic (created by the AI)
    ├── <topic-name>/
    │   ├── INDEX.md  ← index of all notes in this topic
    │   └── *.md      ← individual note files
```

## When to use the notes module

- Use the notes module when the user says to **store notes**, **save notes**,
  or **make notes** about something discussed.
- The AI decides the descriptive topic directory name based on what was discussed.
- The user may specify the topic name explicitly (e.g. "store notes about the SRS").
- Notes are written by the AI — never by the human.

## Naming conventions

### Topic directories — topics/<topic-name>/
- Kebab-case lowercase: `fyp-documents`, `noir-zk`, `presentation-prep`.
- 1 to 4 words max. Never reuse a name; if the topic exists, add notes to it.

### Note files
- Kebab-case: `srs-terms.md`, `viva-checklist.md`.
- Metadata block at the top:

```
---
source: <Q-file(s) or discussion reference>
date: YYYY-MM-DD
model: <model used>
agent: <agent used>
topic: <topic directory name>
---
```

### The Two-File Rule (MANDATORY)
Every topic directory MUST contain `INDEX.md` + at least one note `.md` file.
- Never create a topic directory without its INDEX.md.
- Never create a topic directory with only an INDEX.md and no note files.

## Workflow — "store notes" or "make notes"
1. Determine the topic from the request or the discussion content.
2. Check if `notes/topics/<topic-name>/` exists — if yes, use it; if no, create
   it with its `INDEX.md`.
3. Write the note file(s) with metadata blocks.
4. Append a row to the topic's `INDEX.md` for each note.
5. Update the root `notes/INDEX.md` (add the topic row or increment Note Count).
6. Run `graphify update .`.

## Index maintenance rules — strict

> **FORCED (see `index_entry_rules.md`):** every topic INDEX.md row and every
> root row must have ALL fields filled with real data — no empty cells, no `—`,
> no `TBD`, real ≤ 15-word summaries. A note is NOT done until both its topic
> row and the root registration exist, complete.

### Root notes INDEX.md
- Columns: `| Topic Directory | Created | Note Count | Description |`
- Append-only. `Note Count` = actual number of .md files in the topic directory.

### Topic INDEX.md
- Columns: `| File | Date | Model | Agent | Summary |`
- Append-only; updated every time a note is added.

## Append-only rule
- Existing notes are never edited or deleted.
- To update a topic, write a NEW note file with a new name.

## Relationship to other modules
- Notes are distilled from chat-history Q/A exchanges, but are NOT copies.
- Notes are NOT guides (guides = compiled standalone how-to documents in guides/).

## Error Handling
- If `notes/INDEX.md` is missing: stop and report.
- If a topic directory name is ambiguous: ask the user to clarify.
- If a note file name collides: use a new name (add a suffix).
- If a topic directory exists without INDEX.md (two-file rule violated):
  report the anomaly and create the missing INDEX.md before adding notes.

## Cross-Module Protection
- Notes module is READ-ONLY visible to all other modules.
- Only the notes module writes into `notes/`.
