# App Module — Instructions

## Purpose
`app/` holds the codebase for THIS project's idea. Different from the FYPCE
engine pattern: there is no registry of many apps — this directory is exactly
ONE codebase for exactly ONE idea.

## The one rule
**One idea, one codebase, one directory.** If a second idea needs building, it
gets its own repo (from the kit), its own context engine, its own `app/`.
Never nest a second codebase here, never park scripts for other ideas here.

## Directory layout (fill in as the codebase grows)
```
app/
├── README.md        ← the app's own doc: what it is, how to run it
└── <codebase>       ← the actual project code (any stack the 4 docs specify)
```

## Rules
- The stack, architecture, and contracts come from
  `deliverables/codebase-guidance/` (the 4 docs). The docs lead, the code
  follows. Deviations require updating the 4 docs first, then the code.
- Every code change is committed with a clear message; the contribution
  record for `codebase-development` lists the relevant commits.
- The graphify graph indexes this code automatically — `graphify update .`
  from the repo root after significant changes (never inside `app/`).
- No secrets in code. Environment variables live in `.env` (gitignored) and
  are documented in `app/README.md`.
- `node_modules/`, build output, and caches are gitignored.
