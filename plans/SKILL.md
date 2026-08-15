---
name: plans
description: Read and write numbered plan files in repos that use a plans/ directory with an index. Use when asked to find, list, read, or search plans; when creating a plan or changing one's status; and in place of superpowers writing-plans where the repo declares `superpowers: override`. Triggers on "check the plans", "what plans exist", "write a plan for X", "mark plan 042 completed".
---

# Numbered Plans

A plan system where every unit of work is a numbered markdown file, and one
index lists all of them.

## First: read the repo's parameters

This skill describes the shared convention. Each repo declares its own settings
in a `## Plans` block in its `AGENTS.md` or `CLAUDE.md`:

```markdown
- plans dir:   `plans/`
- index:       `plans/000-INDEX.md`
- statuses:    designing, planned, in-progress, completed, deferred, superseded
- artifacts:   `plans/artifacts/`
- superpowers: override
```

Read that block before anything else. If the repo has no such block, it does
not use this system — do not create one uninvited.

## A plan is the spec

Not a summary that points at one. The design lives in the plan file in full —
problem, decisions and the alternatives rejected, architecture, data flow,
error handling, testing, risks — and stays there as work proceeds, gaining
progress notes and findings.

There is no separate design document to keep in sync, and no two-hop read to
find out what was decided.

## Finding a plan

**Read the index first.** It is the only list of what is tracked. If a file
isn't in the index, it isn't a tracked plan.

Do not glob the plans directory to enumerate plans — artifacts and untracked
files live nearby, and you will read things that are not plans.

## Anatomy

`plans/NNN-name.md`, three-digit zero-padded number, lowercase slug.

```yaml
---
name: Human-readable name
status: designing
created: 2026-08-12
completed: 2026-08-14      # only when status is `completed`
description: >
  Two to four sentences. What the plan covers, what it depends on, what it
  explicitly excludes. Wrapped at 80 columns.
---
```

- **Field order is fixed:** `name`, `status`, `created`, `completed`,
  `description`.
- **`completed` carries the date the plan reached `completed`**, and appears
  only then. Not used for `deferred` or `superseded`. Omit the field rather
  than leaving it bare — a lone `completed:` reads as "completed, date
  unknown" when it means "not completed."
- **`description` uses a folded block (`>`)**, wrapped at 80 columns.
- **Quote `name` only when YAML requires it** — a `: ` inside the name, or a
  leading `>`, `|` or `#`.
- **Extra fields are allowed.** If a repo uses one routinely it will be
  declared in its parameter block.

## Two descriptions, two jobs

They are different fields with different contracts. The index cell is **not** a
copy of the frontmatter description.

| | Length | Job |
|---|---|---|
| Frontmatter `description` | 2–4 sentences | The fuller abstract: scope, dependencies, exclusions |
| Index description cell | **One sentence, 15–30 words** | Let a reader decide whether to open the file |

The index cell carries no status, no progress, no measurements and no design
reasoning. All of that lives in the plan file, the only place it can be kept
current. A cell that restates the plan makes the index unscannable, which is
the one thing it exists to be.

Where this rule is written down it is followed and index descriptions average
23 words; where it isn't, they drift past 150. Keep them short.

## Statuses

The default vocabulary. A repo's parameter block may declare a different set —
use whatever it declares.

| Status | Meaning |
|---|---|
| `designing` | Open questions remain |
| `planned` | Designed, not started |
| `in-progress` | Started; the plan file says what's left |
| `completed` | Shipped, including pieces explicitly deferred to a later plan |
| `deferred` | Deliberately not now |
| `superseded` | Replaced — the index row and the frontmatter both name the replacement |

## Creating a plan

1. **Pick the number:** the highest in the plans *directory* plus one — not the
   highest in the index. Untracked files must not cause a collision.
2. Write `plans/NNN-name.md` with complete frontmatter.
3. Add its index row **in the same change**. A plan without a row is not
   tracked.

## Changing a status

Status is stored **twice** — in the plan's frontmatter and in its index row.

1. Change the frontmatter.
2. Change the index row.
3. **Re-read both and confirm they agree.**

Step 3 is not optional. Skipping it is how an index comes to advertise shipped
work whose plan file still says it was never started.

## Artifacts are not plans

Execution scripts, ledgers, diagnoses, measurement output, benchmarks,
reproducers, pitches and decision records go in the artifacts directory, named
plan-number-first so a listing groups by owner:

```
plans/
  000-INDEX.md
  042-organize-engine.md
  artifacts/
    042-organize-engine-execution.md
    046-vec-delete-repro.cpp
```

- **Committed.** They are the evidence behind decisions recorded in completed
  plans — a reproducer is what lets a bug be re-verified later, a ledger is
  what shows where execution diverged from the plan.
- Owned by exactly one plan, and linked from it.
- Never carries a status, and never gets an index row.

Anything directly in the plans directory is a tracked plan with a row. That
makes "is this in the index?" a question with a real answer.

**Scratch is a third thing.** Intermediate data for a single session belongs in
a gitignored path — not a dot-prefixed file, which lands in `git status` as
untracked noise forever.

## Superpowers

Applies only where the repo declares `superpowers: override`. The workflow is
unchanged; only the output locations move.

| Skill | Default | Here |
|---|---|---|
| `brainstorming` | `docs/superpowers/specs/YYYY-MM-DD-name.md` | `plans/NNN-name.md`, plus an index row |
| `writing-plans` | `docs/superpowers/plans/YYYY-MM-DD-name.md` | `plans/artifacts/NNN-slug.md`, linked from its plan |

So the plan is the `brainstorming` output and the artifact is the
`writing-plans` output for that same plan.

## Completed plans

Do not edit a completed plan unless asked; adding cross-references to other
plans is fine. A change to a shipped feature gets a new plan rather than an
edit to the original.

Always write a plan and get approval before implementing.
