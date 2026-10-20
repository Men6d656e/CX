# Deliverables Module — Instructions

## Purpose
`deliverables/` holds the document work products of this project. All documents
start as Markdown in this repo; `.docx` / `.pptx` exports are produced manually
with any office tool and parked beside their `.md` source. The `.md` file is
the source of truth.

## Directory layout
```
deliverables/
├── proposal/
│   ├── proposal.md            ← the deal proposal (built from the selected IDEA-NNN)
│   └── ppt/                   ← teacher-required presentation for the proposal
│       └── README.md
├── codebase-guidance/         ← THE 4 DOCS — the 0→100% guide for the developer
│   ├── 01-srs.md              ← software requirements specification
│   ├── 02-sdd.md              ← software design document
│   ├── 03-test-plan.md        ← test plan
│   ├── 04-api-data.md         ← API + data model reference
│   └── README.md
└── final-docs/
    ├── documentation.md       ← final FYP documentation
    └── ppt/                   ← final presentation
        └── README.md
```

## Who builds what (FYP Desk flow)
| Step | Deliverable | Who |
|------|-------------|-----|
| 1 | `proposal/proposal.md` | any member (idea must be in fyp-ideas `IDEAS_INDEX.json`) |
| 2 | `proposal/ppt/` | any member |
| 3 | `codebase-guidance/` (4 docs) | team lead by default; any member who has learned the process may take it |
| 4 | `app/` codebase | any member (guided strictly by the 4 docs) |
| 5 | `final-docs/` | any member |

Every completed step appends one contribution record (see
`contribution_history_instructions.md`).

## The 4 codebase-guidance docs — quality bar
These 4 documents are the complete guide from 0% to a finished codebase. A
developer who reads ONLY these 4 files must be able to build the app without
asking the author anything. If a question still needs to be asked, the docs
are incomplete — fix the docs, don't answer privately.

- **01-srs.md** — what to build and for whom: users, features, acceptance
  criteria, out-of-scope list.
- **02-sdd.md** — how it is structured: architecture, modules, diagrams,
  technology choices with reasons.
- **03-test-plan.md** — how we know it works: test cases, expected results,
  edge cases, acceptance pass/fail.
- **04-api-data.md** — the concrete contracts: endpoints, request/response
  shapes, database schema, environment variables.

## Rules
- Markdown first. Never edit an exported `.docx`/`.pptx` as the source —
  edit the `.md` and re-export.
- Never delete a deliverable. Superseded versions stay with a `SUPERSEDED`
  note at the top and a pointer to the replacement.
- Every file created here gets its index row in `tasks-history/INDEX.md`
  (or the module INDEX) in the same turn — `index_entry_rules.md` applies.
- Completing a deliverable without a contribution record is an incomplete task.
