# Apps Module — Instructions

## Purpose
`apps/` holds the codebases for this engine. Each app is its own git repo,
added under `apps/<repo-name>` as a **git submodule** with its own remote and
commit history. This is the ONLY place in the engine where code lives.

## The one rule
**Code lives only in `apps/`, and every entry in `apps/` is its own repo.**
The context engine repo never contains application source directly — it
records the context (history, notes, guides, graph) around the code.

## Directory layout
```
apps/
├── README.md        ← lists every app: name, remote URL, purpose, how to run
└── <repo-name>/     ← one git submodule per application
```

## Adding an app
1. `git submodule add <repo-url> apps/<repo-name>` from the repo root.
2. Update `apps/README.md` with the app's name, remote URL, purpose, and how
   to run it.
3. Run `graphify update .` from the repo root so the graph indexes the new
   code.

## Rules
- Each app keeps its own repo, its own remote, and its own commit history —
  commit inside the submodule, not into the engine repo.
- The engine repo only tracks the submodule pointer (gitlink), never the
  app's source files.
- Work on an app's code is tracked as a task in `tasks-history/` like any
  other task.
- The graphify graph indexes the submodule code automatically —
  `graphify update .` from the repo root after significant changes (never
  inside a submodule).
- No secrets in code. Environment variables live in `.env` (gitignored) and
  are documented in the app's own README.
- `node_modules/`, build output, and caches are gitignored.
