# ARCHITECTURE.md — <FYP-idea-NN>

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
            │  contribution-history/ ──feed──►  valuation  │
            │        ▲                          (org repo) │
            │  deliverables/  app/  notes/  guides/        │
            │        └──────────┬───────────────────────────│
            │                   ▼                          │
            │            graphify-out/graph.json           │
            │        (ONE graph: context + app code)       │
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
4. **Work layer** — `deliverables/` (documents) and `app/` (the single
   codebase) hold the actual outputs. `app/` contains exactly one idea's
   codebase — no second idea, no unrelated code.
5. **Proof layer** — `contribution-history/` is append-only proof of real
   work. Its `index.md` powers the org-wide `contribution-tracker-valueation`
   valuation and pay split. Nothing else qualifies as proof.
6. **Graph layer** — graphify indexes the context files AND `app/` code into
   one graph, so `graphify query` finds related work before any file listing.

## Cross-repo wiring (FYP Desk org)

| Repo | This repo consumes | This repo produces |
|------|--------------------|--------------------|
| `fyp-ideas` | IDEA-NNN identity, selected idea | status updates via team lead |
| `fyp-env-setup-development-team` | this kit (origin of the repo) | — |
| `contribution-tracker-valueation` | — | contribution-history feed files |
| `specilized-agents-skills-from-agency-agents` | specialist agent markdown via its `/api/agent` gateway | — |
