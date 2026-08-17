# plans

A convention for repos where every unit of work is a numbered markdown file and
one index lists all of them. Prose only — nothing to install beyond the skill
file, no script to run.

`SKILL.md` holds the convention itself. This README covers wiring it into a
repo and changing its settings.

## Install

Copy the directory into your agent's skills folder:

```bash
cp -r plans ~/.claude/skills/plans
```

## Turn it on in a repo

The skill does nothing until a repo opts in. Add a `## Plans` block to the
repo's `AGENTS.md` or `CLAUDE.md`:

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

Those are the defaults. A repo with no such block is not using the system, and
the skill will not create one uninvited.

The block does two jobs: it holds the settings, and because it lives in a file
that is always in context it names the skill for agents that never read
`SKILL.md`, such as Codex and Cursor.

## Using it

With the block in place, ask in plain language:

- "check the plans", "what plans exist" — reads the index rather than globbing
  the directory
- "write a plan for X" — creates `plans/NNN-name.md` and its index row in the
  same change
- "read plan 042" — opens the file, which is the spec, not a pointer to one
- "mark plan 042 completed" — updates the frontmatter and the index row, then
  re-reads both to confirm they agree

## Settings

| Key | Default | Controls |
|---|---|---|
| `plans dir` | `plans/` | Where tracked plan files live. Everything directly in it has an index row. |
| `index` | `plans/000-INDEX.md` | The list of tracked plans. A file not listed here is not tracked. |
| `statuses` | designing, planned, in-progress, completed, deferred, superseded | The allowed values for `status`, in frontmatter and in the index row |
| `artifacts` | `plans/artifacts/` | Execution scripts, ledgers, benchmarks, reproducers. Owned by one plan, never given a status or an index row. |
| `superpowers` | — | `override` sends `brainstorming` output to `plans/NNN-name.md` and `writing-plans` output to an artifact. `default` leaves both alone. |

Frontmatter takes extra fields. Declare one in the block if the repo uses it
routinely.

## Overriding

Every key can be changed. This repo keeps plans under `docs/plans/`, names the
index differently, uses four statuses of its own, and leaves superpowers on its
default behaviour:

```markdown
## Plans

This repo uses the numbered-plan system — see the `plans` skill for how to
read and write one.

- plans dir:    `docs/plans/`
- index:        `docs/plans/INDEX.md`
- statuses:     draft, active, completed, rejected
- artifacts:    `docs/plans/artifacts/`
- superpowers:  default
- extra fields: `superseded_by`
```

Anything the block does not mention keeps its default. Changing `statuses`
replaces the vocabulary rather than adding to it — plan frontmatter and index
rows both have to use the declared set.
