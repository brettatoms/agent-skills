# Plan System Skill — Design

Date: 2026-08-15
Status: approved, not yet implemented

## Summary

Four repos — sepal, phig, jax, banzai — use the same numbered-plan system, and
each documents it separately in its own `AGENTS.md` or `CLAUDE.md`. The copies
have drifted. This design consolidates the convention into one skill,
`~/devel/agent-skills/plans/SKILL.md`, and reduces a repo to a short parameter
block naming its own settings.

**Only sepal adopts it for now**, and sepal's existing plan files are migrated
to match. phig, jax and banzai are unchanged; the evidence gathered from them
still justifies the design and is kept below.

This is the first of two deliverables. The second — an Emacs package that lists
and opens plans from the index — is designed separately and depends on nothing
here.

## Problem

The convention is duplicated four times and is decaying in measurable ways.

**The prose is copy-pasted.** `sepal/AGENTS.md` and `phig/CLAUDE.md` carry the
same paragraphs verbatim: the plan-is-the-spec framing, the six status glosses,
"If it isn't in the index, it isn't tracked", and the index-row contract.
Roughly 40 lines maintained in two places.

**One copy is already wrong.** `jax/AGENTS.md` tells agents to read and update
`plans/INDEX.md` at lines 64 and 139. The file is `plans/000-INDEX.md`; no
`INDEX.md` exists. Both instructions produce a failed read.

**Rules hold only where they are written.** The index description is specified
as one sentence of 15–30 words in sepal and phig, and nowhere in jax or banzai:

| Repo | Rule stated | Median words | Mean | Max | Over 30 words |
|---|---|---|---|---|---|
| sepal | yes | 23 | 23 | 27 | 0% |
| phig | yes | 27 | 27 | 42 | 16% |
| jax | no | 33 | 42 | 163 | 54% |
| banzai | no | 40 | 54 | 197 | 59% |

**Index and plan files disagree.** Status is stored twice — in the plan's
frontmatter and in its index row — and five pairs have diverged:

```
jax    089-clojure-inspector          index=planned    frontmatter=in-progress
banzai 061-banzai-worker-v2-migration index=completed  frontmatter=draft
banzai 098-manager-add-users          index=completed  frontmatter=draft
banzai 116-cms-contact-detail-page    index=draft      frontmatter=completed
banzai 086-banzai-warehouse-migration index=draft      frontmatter=active
```

Two banzai rows advertise shipped work whose own plan file says `draft`.

**Frontmatter varies at the edges.** Across 314 plans, `name, status, created,
description` appears in that order with a bare `YYYY-MM-DD` date in 100% of
files. Everything else differs:

| | sepal | phig | jax | banzai |
|---|---|---|---|---|
| `description` folded `>` | 10/10 | 64/65 | 110/110 | 7/129 |
| `completed` field | absent | 2 files | 101 files, 22 empty | 33 files |
| `completed` position | — | after `status` | after `created` | after `created` |
| `name` quoted | 0 | 0 | 0 | 106/129 |
| one-off fields | — | `closes_rules`×8, `superseded_by` | `updated`×3, `feature-branch` | `rejected`×2 |

One jax file lists `completed:` twice.

**Artifacts have no enforced home.** 19 files sit in `plans/` without index
rows — `-impl.md`, `-pitch.md`, `-decisions.md`, `-log.md`. They are execution
artifacts, not tracked plans, but nothing stops them landing next to plans, so
"is this file in the index?" has no reliable answer.

## Scope

In scope: the skill; sepal's `AGENTS.md` parameter block; and migrating sepal's
existing plan files and artifacts to match.

Out of scope: any tooling (explicitly rejected — prose only); phig, jax and
banzai, including their `AGENTS.md`/`CLAUDE.md` files and the data problems
listed at the end; the Emacs package.

Adopting the skill in one repo first means the convention gets exercised
against real files before three more repos depend on it. sepal is the right
first repo: it is the smallest (10 plans), the only one with no index drift,
and already conforms to the frontmatter standard in every respect but one.

## Decisions

1. **The skill owns the convention.** Each repo keeps a parameter block naming
   its settings and pointing at the skill. All four repos migrate.
2. **Prose only.** No lint script, no CLI, nothing to install. Reading and
   writing plans works in any agent in any repo immediately.
3. **A canonical status vocabulary with per-repo override.** sepal, phig and
   jax converge on the six; banzai keeps its four.
4. **The superpowers override is per-repo**, not baked into the skill.
5. **Artifacts live in `plans/artifacts/`**, named plan-number-first.
6. **Frontmatter is standardized** for new and edited plans; existing files are
   not retro-fixed.

## The skill

Location: `~/devel/agent-skills/plans/SKILL.md`. Single file, no `references/`.
Entry added to the repo's root `README.md`. Agent-generic — not flagged
Claude-specific.

The name `plans` overlaps conceptually with superpowers' `writing-plans` and
`executing-plans`. Those are namespaced `superpowers:*` in the skill listing,
and this skill's description states the override explicitly, so the collision
is tolerable.

### Frontmatter

```yaml
---
name: plans
description: Read and write numbered plan files in repos that use a plans/
  directory with an index. Use when asked to find, list, read, or search plans;
  when creating a plan or changing one's status; and in place of superpowers
  writing-plans where the repo declares `superpowers: override`. Triggers on
  "check the plans", "what plans exist", "write a plan for X", "mark plan 042
  completed".
---
```

### Auto-invocation

Two legs, because a skill alone is discretionary:

- **The description above** fires on plan-shaped requests.
- **The parameter block in each repo** is always in context and names the
  skill. This is the reliable leg, and it is the only one that works in Codex
  or Cursor, where `SKILL.md` is never read.

### Contents

1. **A plan is the spec.** Not a summary pointing at one. Problem, decisions
   and rejected alternatives, architecture, data flow, error handling, testing,
   risks — all in the plan file, gaining progress notes as work proceeds. No
   separate design document to keep in sync.
2. **Finding plans.** Read the repo's parameter block for the plans directory
   and index filename, then read the index. Do not glob the plans directory;
   the index is the list of what is tracked.
3. **Anatomy.** `plans/NNN-name.md`, three-digit numbering, frontmatter per the
   standard below.
4. **The two descriptions.** Frontmatter description and index description cell
   are different fields with different jobs. See below.
5. **Creating a plan.** The next number is the highest in the *directory* plus
   one, not the highest in the index, so untracked files cannot cause a
   collision. Write the plan file and its index row in the same change.
6. **Updating status.** Status is stored twice. Change both, then re-read both
   and confirm they agree before finishing. With no tooling, this is the only
   guard against the five known mismatches recurring.
7. **Statuses.** The six defaults, with the glosses below; the repo's block may
   override the set.
8. **Artifacts are not plans.** See below.
9. **Superpowers.** Where the repo declares `superpowers: override`, a
   `brainstorming` spec becomes the plan and a `writing-plans` execution script
   becomes an artifact. Otherwise this section does not apply.
10. **Completed plans.** Do not edit unless asked; cross-references are fine.
    Changes to shipped features get a new plan.

## The parameter block

Replaces the plan prose in each repo's `AGENTS.md` or `CLAUDE.md`:

```markdown
## Plans

This repo uses the numbered-plan system — see the `plans` skill for how to
read and write one.

- plans dir:   `plans/`
- index:       `plans/000-INDEX.md`
- statuses:    designing, planned, in-progress, completed, deferred, superseded
- artifacts:   `plans/artifacts/`
- superpowers: override
```

`superpowers: override` redirects both superpowers outputs, each to a different
place. The workflow is unchanged; only the locations move.

| Skill | Default location | Under `override` |
|---|---|---|
| `brainstorming` | `docs/superpowers/specs/YYYY-MM-DD-name.md` | `plans/NNN-name.md`, plus an index row |
| `writing-plans` | `docs/superpowers/plans/YYYY-MM-DD-name.md` | `plans/artifacts/NNN-slug.md`, linked from the plan that owns it |

This is the cleanest statement of what an artifact is: the plan is the
`brainstorming` output, the artifact is the `writing-plans` output for that
same plan. The two-way split is taken from sepal's own working copy, which
states it better than the version this design started from.

`superpowers: default` means superpowers behaves normally and the plan system
is used independently.

## Statuses

The default vocabulary, taken from sepal and phig, which already share it:

| Status | Meaning |
|---|---|
| `designing` | Open questions remain |
| `planned` | Designed, not started |
| `in-progress` | Started; the plan file says what's left |
| `completed` | Shipped, including pieces explicitly deferred to a later plan |
| `deferred` | Deliberately not now |
| `superseded` | Replaced — the index row and the frontmatter both name the replacement |

jax converges on these at no cost: its three (`planned`, `in-progress`,
`completed`) are a strict subset, so no existing file changes — the allowed set
just widens.

banzai keeps `draft`, `active`, `completed`, `rejected` in its parameter block.
Converging it would mean rewriting frontmatter and index rows across 62 plans
in a shared work repo; that stays a separate decision.

## Frontmatter standard

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
  only then. It is not used for `deferred` or `superseded`. Omit the field
  entirely rather than leaving it bare: a lone `completed:` reads as
  "completed, date unknown" when it means "not completed."
- **`description` uses a folded block (`>`)**, wrapped at 80 columns.
- **Quote `name` only when YAML requires it** — a `: ` within the name, or a
  leading `>`, `|` or `#`. Quote when needed, not always and not never.
- **Extra fields are allowed.** If a repo uses one routinely, declare it in the
  parameter block.

## The two descriptions

**Frontmatter `description`** — the fuller abstract. Two to four sentences:
scope, dependencies, exclusions.

**Index description cell** — a condensation. One sentence, 15–30 words. Its
only job is to let a reader decide whether to open the file. It carries no
status, no progress, no measurements and no design reasoning; all of that lives
in the plan file, the only place it can be kept current. A cell that restates
the plan makes the index unscannable, which is the one thing it exists to be.

The index cell is not a copy of the frontmatter description. sepal 003 is the
model: a 52-word frontmatter description condensed to a 24-word cell.

Note for the migration: sepal and phig currently say an index *row* "carries no
status" while their tables have a Status column. It means the description cell.
The skill says "description cell" to remove the contradiction.

## Artifacts

Execution scripts, ledgers, diagnoses, measurement output, benchmarks,
reproducers, pitches and decision records go in `plans/artifacts/`.

```
plans/
  000-INDEX.md
  042-organize-engine.md
  artifacts/
    038-live-photo-pairing.md
    038-make-mov-fixture.sh
    045-rowid-reuse-bench.cpp
    046-vec-delete-repro.cpp
    048-heic-decode-bench.c
```

Named `NNN-slug.ext`, plan number first, so a listing groups by owning plan.

Rules:

- **Committed.** They are the evidence behind decisions recorded in completed
  plans. `046-vec-delete-repro.cpp` is what allows that bug to be re-verified
  later; a ledger is what shows where execution diverged from the plan.
- Owned by exactly one plan, and linked from it.
- Never carries a status of its own, and never gets an index row.

Anything directly in `plans/` is a tracked plan and has an index row. That
makes "is this file in the index?" a question with a real answer.

**Scratch is a third category.** Intermediate data for a single session —
banzai's `.117-baseline-depgraph.edn` and friends — belongs in a gitignored
path, not a dot-prefixed file. Those four are currently untracked *and*
un-ignored, so they sit in `git status` as noise indefinitely.

## Migration — sepal only

**Precondition: sepal's working tree must be clean.** At the time of writing it
carries ~615 insertions and ~641 deletions across 10 files, including an
in-flight rewrite of the very `AGENTS.md` section this replaces, and two design
docs staged for deletion whose content is being folded into plans 002 and 006.
Every step below touches a file in that diff. Commit or stash first.

1. **`AGENTS.md`** — replace the Plans section with the parameter block:

   ```
   - plans dir:   `plans/`
   - index:       `plans/000-INDEX.md`
   - statuses:    designing, planned, in-progress, completed, deferred, superseded
   - artifacts:   `plans/artifacts/`
   - superpowers: override
   ```

   Also update the layout table's `docs/` row, which currently describes it as
   the artifact directory.

2. **Add `completed:` to the two completed plans.** Neither records a date
   today, and it exists nowhere else in the repo:

   | Plan | Date | Source |
   |---|---|---|
   | `001-workspace-restructure.md` | 2026-08-12 | landed already-completed in the init commit, so this is the repo's creation date, not necessarily the work's — needs confirming |
   | `006-multi-instance-spike.md` | 2026-08-14 | commit `fe60706`, which added the plan with its findings |

3. **Move the two execution artifacts** into `plans/artifacts/`, renamed
   number-first:

   ```
   docs/2026-08-13-multi-instance-spike-execution.md
     → plans/artifacts/006-multi-instance-spike-execution.md
   docs/2026-08-14-multi-instance-library-execution.md
     → plans/artifacts/002-multi-instance-library-execution.md
   ```

4. **Update the links** to them in `plans/002-multi-instance-library.md:16` and
   `plans/006-multi-instance-spike.md:14`, which currently point at `../docs/`.

5. **Leave `docs/legacy-plans/` alone.** Its 22 files are an archive of
   pre-numbering plans owned by no plan, so they are not artifacts. Moving them
   would also require editing `001`, a completed plan, which the convention
   forbids. `docs/` survives in sepal for this and nothing else.

6. **Re-wrap `009` and `010`** to 80 columns. Both otherwise conform.

Plus a one-line entry in `~/devel/agent-skills/README.md`.

No other plan file needs changing: sepal's 10 plans already use the correct
field order, folded descriptions, bare dates and unquoted names, and its index
descriptions already average 23 words.

## Known data problems in the other three repos

Not addressed here — phig, jax and banzai are out of scope. Recorded so they
are not lost if those repos adopt the skill later:

- Five index/frontmatter status mismatches (listed under Problem).
- 19 artifact files sitting in `plans/` without index rows: 3 in phig, 4 in
  jax, 12 in banzai.
- banzai `085-rds-cutover-schedule.md` has no frontmatter.
- One jax plan lists `completed:` twice.
- banzai's four dot-prefixed scratch files, untracked and un-ignored.

## Follow-on

The Emacs package that lists and opens plans from the index. Decided so far:
records come from the index table with a configurable filename, and per-project
configuration lives in `.dir-locals.el` at each workspace root. Designed
separately.
