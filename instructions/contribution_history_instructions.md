# Contribution-History Module — Instructions

## Purpose
`contribution-history/` is the **append-only proof-of-work feed** of this
project repo. Every real work item — proposal, PPT, the 4 codebase-guidance
docs, codebase development, final documentation, final PPT — appends exactly
one file here when it is completed and pushed. An LLM agent scans this
directory, reads `index.md`, and imports the work records into
[contribution-tracker-valueation](https://github.com/FYP-DESK/contribution-tracker-valueation),
which computes who did what and who deserves what share of the fee.

> **The gate rule:** if the work item is not finished and pushed, its
> contribution file does not exist. If a contribution file exists, the work it
> describes must be verifiable in this repo (the `evidence` field points at it).
> No contribution file = untracked work = no valuation, no pay.

## Directory layout
```
contribution-history/
├── index.md            ← master index: every contribution file, one row each
├── c001.md             ← first contribution record
├── c002.md             ← next (zero-padded, never reused)
└── c(NNN).md           ← …
```

## What counts as a contribution (real work only)
| Work type | Where the evidence lives |
|-----------|--------------------------|
| `proposal` | `deliverables/proposal/*.md` (+ exported .docx) |
| `proposal-ppt` | `deliverables/proposal/ppt/` |
| `codebase-guidance-docs` | `deliverables/codebase-guidance/` — all 4 docs |
| `codebase-guidance-doc` | one of the 4 docs (partial contribution) |
| `codebase-development` | `app/` commits (commits list at least one) |
| `final-documentation` | `deliverables/final-docs/*.md` |
| `final-ppt` | `deliverables/final-docs/ppt/` |
| `environment-setup` | this repo's initial kit setup (once per project) |

**Never** append contribution files for: questions, answers, chat exchanges,
task tracking, notes, guides, reading, review, or discussions. Those live in
the instance modules. If you are unsure whether something is "real work", it
is not.

## Record format — c(NNN).md

```markdown
---
id: c001
date: 2026-09-28
contributor: <member-id from ids.json, e.g. akash>
work_type: proposal
idea_id: IDEA-NNN
deal_id: <deal id or "unassigned">
status: complete
---
# c001 — Proposal for <idea name>

## What was done
One short paragraph, factual.

## Evidence
- `deliverables/proposal/proposal.md` (committed in <commit-sha or "this push">)

## Hours (optional, self-reported)
~4h
```

Frontmatter fields — ALL required, ALL real:
- `id` — c(NNN), zero-padded to 3 digits, never reused.
- `date` — YYYY-MM-DD of completion.
- `contributor` — the member id; must exist in the org registry
  `contribution-tracker-valueation/data/ids.json`.
- `work_type` — one value from the table above.
- `idea_id` — IDEA-NNN from fyp-ideas `IDEAS_INDEX.json`.
- `deal_id` — the deal this work belongs to, or `unassigned`.
- `status` — `complete` (the only value that counts for valuation).

## index.md format

One row per contribution file:

```markdown
| File | Contributor | Work type | Idea | Date | Status | Summary |
|------|-------------|-----------|------|------|--------|---------|
| c001.md | akash | proposal | IDEA-001 | 2026-09-28 | complete | Drafted full proposal md |
```

## Workflow — completing a real work item
1. Do the work in `deliverables/` or `app/`, commit, push.
2. Create the next `contribution-history/c(NNN).md` with the frontmatter above.
3. Append its row to `contribution-history/index.md`.
4. Append index rows for the work files themselves (per `index_entry_rules.md`).
5. Run `graphify update .`.
6. Push everything together.

## Valuation import (how the tracker eats this)
An agent (or the tracker's import script) reads `index.md`, filters
`status: complete`, cross-checks evidence paths exist, then writes records into
`contribution-tracker-valueation/data/contributions.json`. The tracker never
modifies this directory — it is read-only for everyone except the contributor
appending their own record.

## Fairness rules
- You append ONLY your own records. Never touch another member's file.
- One work item = one record. Do not split a proposal into 5 records.
- The 4 codebase-guidance docs may be recorded as one `codebase-guidance-docs`
  record or four `codebase-guidance-doc` records — by the person who made them.
- Self-reported hours are optional context, never the pay basis. The basis is
  the work slice weights in the tracker's `data/splits.json`.
- Disputes are resolved by evidence: the `Evidence` section must point at
  real committed files.
