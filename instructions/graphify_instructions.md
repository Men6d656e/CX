# Graphify Module — Instructions

## Purpose
Graphify provides a knowledge graph of the engine at `graphify-out/`. It indexes
files as nodes, extracts relationships as edges, and identifies communities.
This file is the ONLY set of rules for the graphify module.

## Directory Contents

| File | Purpose | When to Use |
|------|---------|-------------|
| `graph.json` | Full graph data (nodes + edges) | Programmatic access, graphify query results |
| `graph.html` | Visual graph representation | Visual exploration of structure |
| `GRAPH_REPORT.md` | Community structure report | Broad architecture review |
| `.graphify_chunk_*.json` | Intermediate extraction artifacts | Internal — ignore, will be regenerated |

## Commands

```
graphify query "<question>"     # codebase/context questions — scoped subgraph
graphify path "<A>" "<B>"       # relationships between two components/files
graphify explain "<concept>"    # focused explanation of a concept
graphify update .               # refresh the graph (AST-only, no API cost)
```

## MANDATORY — First-step file discovery (every instance)

Before reading ANY target file from the filesystem in ANY instance (1st, 2nd, or
3rd), run a graphify query to locate the relevant files first:

```
graphify query "<the task topic>"
```

- The query returns the most relevant Q/A/T/E/guide/notes/app files with their
  relationships — read THOSE instead of guessing paths or listing directories.
- The filesystem read comes AFTER graphify tells you what is relevant. Directory
  listing and raw grep are FALLBACKS only — used when graphify is unavailable,
  returned nothing, or after `graphify update .` still finds nothing.
- This applies to every instance: locating related past Q/A pairs (1st), related
  past tasks and executions (2nd/3rd), guides, notes, and instruction files.
  This repo's own files ARE indexed in the graph — finding the right context file
  is exactly what graphify is for.
- After the work is done, run `graphify update .` so the new files are indexed.

## Apps submodule — how the graph handles app code (Option A: ONE engine graph)

`apps/fyp-service-app` is a git submodule (its own repo:
`github.com/Akash-Mirza/fyp-service-page`). The graphify strategy is:

- **ONE graph for the whole engine.** `graphify-out/` lives at the repo root
  and indexes EVERYTHING: context files (Q/A/T/E, guides, notes) AND the app
  submodule's source files (HTML/CSS/JS).
- **No separate per-app graph.** Do not create `graphify-out/` inside
  `apps/fyp-service-app/` — the submodule's files are already indexed by the
  engine graph. If a per-app graph is ever wanted (for working on a standalone
  clone of the repo), that is a future decision requiring a new rule here first.
- **Run `graphify update .` from the repo root only** — never from inside the
  submodule directory.
- **Cross-links are the point:** a query like "navbar revision" surfaces BOTH
  the app's code files AND the Q/A/T/E discussions behind them. Use this when
  working on app tasks: find the code and its context in one query.
- App tasks (2nd/3rd instance) end with `graphify update .` from the repo root
  root, so code changes made in the submodule are indexed too.

## Graphify Rules

1. **First resort for file discovery:** When `graphify-out/graph.json` exists,
   run `graphify query` before raw grep, directory listing, or file browsing —
   in EVERY instance, not just codebase questions.
2. **Dirty files are normal:** After hooks or incremental updates, chunk files
   appear. This is expected. Do NOT skip graphify because of dirty files.
3. **Wiki takes priority:** If `graphify-out/wiki/index.md` exists, use it for
   broad navigation instead of raw source browsing.
4. **Graph report is secondary:** Read `GRAPH_REPORT.md` only for broad
   architecture review or when query/path/explain don't surface enough context.
5. **Keep it current:** After modifying files, run `graphify update .` to keep
   the graph current. This is AST-only and has no API cost.
6. **No LLM needed for updates:** `graphify update .` uses AST extraction only.
   Set `GEMINI_API_KEY` or `GOOGLE_API_KEY` only if you want semantic extraction.

## When NOT to Use Graphify

- The user explicitly says not to use graphify
- The task is a pure single-file write whose exact target file the user already
  named (e.g. "edit SYSTEM.md") — graphify adds nothing there. Everything else
  goes through graphify first.
- The question is about file contents that haven't been indexed yet (run update first)

**Never skip graphify because "the task is about context/history" or "the task
is about the app".** Context files and app code are both indexed in the graph;
that is the primary use case in this repo.

## Integration with the repo

- Graphify is a tool layer, not a context engine. It indexes files, not interactions.
- Graphify output (graph.json, graph.html, GRAPH_REPORT.md) is READ-ONLY —
  agents read it, they never write graph files directly.
- The `.opencode/` directory contains the graphify plugin and skill
  (`SKILL.md`) — same setup as MCE and BCE.
- Only run `graphify update .` from the repo root (never inside the submodule).

## Error Handling

### If graphify-out/ doesn't exist
- Run `graphify update .` to build the graph first
- If graphify is not installed, the CLI will guide installation

### If graph.json is missing or empty
- The graph needs to be built: run `graphify update .`
- Do NOT attempt queries on a non-existent graph

### If queries return no results
- The graph may not have indexed the relevant files yet
- Run `graphify update .` to refresh
- Check if the files are in the scanned directory

### If graphify command is not found
- Graphify may not be installed in this environment
- Use `graphify update .` which will auto-install if needed; otherwise report

## Cross-Engine Access

- **READ-ONLY:** Agents from any engine can READ graphify-out/ files
- **WRITE PROTECTION:** Agents MUST NOT write directly to graphify-out/ files
- **Ownership:** graphify-out/ is owned by this repo — only run `graphify update .` from the repo root
