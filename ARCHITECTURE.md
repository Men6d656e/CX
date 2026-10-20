# ARCHITECTURE.md 

## The one diagram

```
            ┌──────────────────────────────────────────────┐
            │              THIS REPO (one idea)             │
            │                                              │
  prompts ──►  chat-history / tasks-history   (history)    │
            │        │                                     │
            │        ▼                                     │
            │  instructions/  ──rules govern──►  modules   │
            │        │                                     │
            │        ▼                                     │
            │                 apps/ notes/  guides/        │
            │        └──────────┬───────────────────────────│
            │                   ▼                          │
            │            graphify-out/graph.json           │
            │        (ONE graph: context + apps code)       │
            └──────────────────────────────────────────────┘
```

## Layers

1. **History layer** — `chat-history/` (1st instance) and `tasks-history/`
   (2nd + 3rd instance) record everything that happens, append-only. Any
   teammate or agent can reconstruct who knew what, when, and why.
2. **Rules layer** — `instructions/` holds one rule file per module. The
   instance module decides where an input lands. Index-entry rules force
   every file to be registered with real metadata.
3. **Knowledge layer** — `notes/` and `guides/` distill consultations and
   how-tos so nobody re-answers the same question.
4. **Work layer** — `apps/` 
5. **Graph layer** — graphify indexes the context files AND `apps/` code into
   one graph, so `graphify query` finds related work before any file listing.
