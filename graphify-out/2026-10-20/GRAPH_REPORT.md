# Graph Report - CX  (2026-10-20)

## Corpus Check
- 18 files · ~11,831 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 184 nodes · 166 edges · 19 communities (14 shown, 4 thin omitted)
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `22f750c7`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- Tasks-History Module — Instructions
- Guides Module — Instructions
- Error Handling
- Notes Module — Instructions
- Chat-History Module — Instructions
- Graphify Module — Instructions
- Utility Module — Instructions
- Hard Rule — This Is Not Optional
- Index Entry Rules — FORCED, applies to every INDEX.md
- SYSTEM.md — CX Context Engine
- Apps Module — Instructions
- CX — Context Engine
- ARCHITECTURE.md
- CX — Index
- chat-history/INDEX.md
- instructions/INDEX.md
- tasks-history/INDEX.md
- 1. What is Node.js?

## God Nodes (most connected - your core abstractions)
1. `Hard Rule — This Is Not Optional` - 13 edges
2. `Graphify Module — Instructions` - 11 edges
3. `Notes Module — Instructions` - 11 edges
4. `Chat-History Module — Instructions` - 10 edges
5. `Guides Module — Instructions` - 10 edges
6. `Tasks-History Module — Instructions` - 10 edges
7. `Utility Module — Instructions` - 9 edges
8. `Index Entry Rules — FORCED, applies to every INDEX.md` - 8 edges
9. `Workflow — 3rd instance (agent's own prompt interface)` - 7 edges
10. `CX — Context Engine` - 6 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Communities (19 total, 4 thin omitted)

### Community 0 - "Tasks-History Module — Instructions"
Cohesion: 0.11
Nodes (18): Append-only rule, Cross-repo protection, Directory layout, Error Handling, Index maintenance, Mandatory steps, Media already inside a referenced T file (2nd instance), Media — user-placed, not agent-saved (+10 more)

### Community 1 - "Guides Module — Instructions"
Cohesion: 0.11
Nodes (17): Cross-Module Protection, Directory layout, Error Handling, Guide files — guides/<filename>.md, Guide metadata block, guides/INDEX.md, Guides Module — Instructions, INDEX.md maintenance — strict (+9 more)

### Community 2 - "Error Handling"
Cohesion: 0.15
Nodes (13): Detection Criteria, Duplicate Detection (MANDATORY), Duplicate Detection Rules, Duplicate Detection Workflow, Error Handling, File Existence Check Workflow (MANDATORY — Hard Gate), Files That Must Be Checked, Graphify-first file discovery (every instance) (+5 more)

### Community 3 - "Notes Module — Instructions"
Cohesion: 0.12
Nodes (16): Append-only rule, Cross-Module Protection, Directory layout, Error Handling, Index maintenance rules — strict, Naming conventions, Note files, Notes Module — Instructions (+8 more)

### Community 4 - "Chat-History Module — Instructions"
Cohesion: 0.12
Nodes (15): A files — outputs/A(N).md, Append-only rule, Chat-History Module — Instructions, Cross-repo protection, Directory layout, Error Handling, Index maintenance, Media files — media/ (+7 more)

### Community 5 - "Graphify Module — Instructions"
Cohesion: 0.12
Nodes (15): Apps submodules — how the graph handles app code (Option A: ONE engine graph), Commands, Cross-Engine Access, Directory Contents, Error Handling, Graphify Module — Instructions, Graphify Rules, If graph.json is missing or empty (+7 more)

### Community 6 - "Utility Module — Instructions"
Cohesion: 0.13
Nodes (14): Adding a new utility, Cross-Module Protection, Directory layout, Error Handling, How to run a utility (mandatory), Naming conventions, Purpose, Relationship to other modules (+6 more)

### Community 7 - "Hard Rule — This Is Not Optional"
Cohesion: 0.12
Nodes (16): Cross-instance contamination, Cross-repo protection, File references — mandatory verification before any work, Hard Rule — This Is Not Optional, How the agent chooses the instance (mandatory decision flow), Instance Module — Instructions, Media — the user places it, not the agent, No guessing the user's intent about the instance (+8 more)

### Community 8 - "Index Entry Rules — FORCED, applies to every INDEX.md"
Cohesion: 0.22
Nodes (8): How this file is enforced, Index Entry Rules — FORCED, applies to every INDEX.md, Rule 1 — Every new file gets an index row (hard gate), Rule 2 — Every field must be filled (no empty cells), Rule 3 — Multi-file tasks get one row per file, Rule 4 — Legacy file names (repair on next touch, do not rename), Rule 5 — Registering a file whose index row was forgotten (repair), Rule 6 — Audit on every module write

### Community 9 - "SYSTEM.md — CX Context Engine"
Cohesion: 0.22
Nodes (8): Forced Rules — never skippable, Graphify-first file discovery, Identity, Index entries — FORCED, Rules, Startup — do this first in every session, SYSTEM.md — CX Context Engine, Validation Checklist

### Community 10 - "Apps Module — Instructions"
Cohesion: 0.29
Nodes (6): Adding an app, Apps Module — Instructions, Directory layout, Purpose, Rules, The one rule

### Community 11 - "CX — Context Engine"
Cohesion: 0.29
Nodes (6): apps/ — code lives here, CX — Context Engine, Members, Modules, Non-negotiable rules (enforced by the instructions), Startup

### Community 13 - "CX — Index"
Cohesion: 0.50
Nodes (3): CX — Index, Flow, Modules

### Community 18 - "1. What is Node.js?"
Cohesion: 0.33
Nodes (5): 1. What is Node.js?, Key facts, Simple mental model, What it's NOT, What it's used for

## Knowledge Gaps
- **133 isolated node(s):** `The one diagram`, `Layers`, `Modules`, `Flow`, `Members` (+128 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 151 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **4 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Error Handling` connect `Error Handling` to `Hard Rule — This Is Not Optional`?**
  _High betweenness centrality (0.015) - this node is a cross-community bridge._
- **Why does `Instance Module — Instructions` connect `Hard Rule — This Is Not Optional` to `Error Handling`?**
  _High betweenness centrality (0.015) - this node is a cross-community bridge._
- **What connects `The one diagram`, `Layers`, `Modules` to the rest of the system?**
  _133 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Tasks-History Module — Instructions` be split into smaller, more focused modules?**
  _Cohesion score 0.10526315789473684 - nodes in this community are weakly interconnected._
- **Should `Guides Module — Instructions` be split into smaller, more focused modules?**
  _Cohesion score 0.1111111111111111 - nodes in this community are weakly interconnected._
- **Should `Notes Module — Instructions` be split into smaller, more focused modules?**
  _Cohesion score 0.11764705882352941 - nodes in this community are weakly interconnected._
- **Should `Chat-History Module — Instructions` be split into smaller, more focused modules?**
  _Cohesion score 0.125 - nodes in this community are weakly interconnected._