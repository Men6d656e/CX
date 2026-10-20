# kit — FYP Desk controlled environment (0–100 template)

Copy this entire `kit/` directory as the base of a new FYP-idea repo, rename the
repo to `<fyp-idea-NN>-<slug>`, delete the outer README.md, and start working.
Everything below is already wired; you only fill the placeholders.

The kit combines the **context-engine pattern** (SYSTEM.md + INDEX + modules +
append-only history + graphify) with FYP-Desk-specific modules:

| Module | Purpose in a project repo |
|--------|--------------------------|
| `chat-history/` | every user↔agent Q/A exchange, word-for-word (instance 1) |
| `tasks-history/` | task definitions, trackers, logs, executions (instances 2+3) |
| `instructions/` | the rule files — one per module, plus the instance decision flow |
| `notes/` | topic-organized knowledge distilled from consultations |
| `guides/` | compiled how-to guides (trigger: any action word + "guide") |
| `contribution-history/` | append-only proof-of-work feed for the valuation tracker |
| `deliverables/` | the actual work products: proposal, PPT, 4 docs, final docs/PPT |
| `app/` | ONE codebase, ONE idea — the only place code is allowed |
| `graphify-out/` | knowledge graph over this repo's context AND app code |
| `utility/` | on-demand runnable procedures |

## The 0→100 flow (who does what)

```
fyp-ideas repo ──idea selected──► THIS repo (kit copy)
                                     │
  [any member] deliverables/proposal/        ← 1. proposal .md (+ .docx later)
  [any member] deliverables/proposal/ppt/    ← 2. teacher PPT
  [team lead or trained member]              ← 3. the 4 codebase-guidance docs
      deliverables/codebase-guidance/           (SRS · SDD · test plan · API/data)
      0% → 100%: everything the developer needs, nothing else
  [any member] app/                          ← 4. the codebase (ONE idea only)
  [any member] deliverables/final-docs/      ← 5. final documentation + PPT
```

Every finished step 1–5 = ONE file appended in `contribution-history/`.
No contribution file = no tracked work = no pay split.

## Activation checklist (one-time, ~5 minutes)

1. Copy `kit/` contents to the new repo root (or rename `kit` and move it up).
2. Delete the outer README.md of the setup repo.
3. Fill the placeholders: repo name + IDEA-NNN in `SYSTEM.md` and `INDEX.md`.
4. `git init`, first commit, push to `FYP-DESK/<fyp-idea-NN>-<slug>`.
5. Run `graphify update .` — the graph must exist before the first task.

## Non-negotiable rules (enforced by the instructions)

- History is append-only: never edit or delete existing Q/A/T/E files.
- Every file created gets its index row in the SAME turn — all fields filled.
- Small Q/A and quick questions go through the instance flow (chat-history /
  tasks-history). Only REAL work items (proposal, PPT, 4 docs, code, docs, PPT)
  append to `contribution-history/`.
- Code lives ONLY in `app/`, and `app/` holds exactly one idea's codebase.
- All documents start as `.md`; office formats (.docx/.pptx) are exported
  manually and parked beside their `.md` source.
