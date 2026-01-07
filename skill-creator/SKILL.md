---
name: skill-creator
description: Create or update Claude skills. Use when user wants to create a new skill, update an existing skill, or asks about skill structure. Handles both user-level skills (portable across projects) and project-level skills (repository-specific).
allowed-tools: WebFetch, Read, Write, Bash, AskUserQuestion, Glob
---

# Skill Creator

This skill wraps Anthropic's official skill-creator with local context.

## Skill Types

**User skills** (`~/.claude/skills/<name>/`):
- Portable across all projects
- General-purpose utilities (file-nav, code-search, github, etc.)
- NOT tied to any specific repository

**Project skills** (`.claude/skills/<name>/` in repo root):
- Repository-specific knowledge and workflows
- Codebase conventions, testing patterns, deployment procedures
- Only available within that project

## Workflow

### 1. Determine Skill Type

Before creating a skill, clarify with the user:
- **User skill**: "Should this skill be available across all your projects?"
- **Project skill**: "Is this specific to the current repository?"

If unclear, ask using AskUserQuestion.

### 2. Fetch Upstream Guidance

For skill creation best practices, fetch Anthropic's skill-creator:

```
WebFetch: https://raw.githubusercontent.com/anthropics/skills/main/skills/skill-creator/SKILL.md
```

For specific patterns:
- **Workflows**: `https://raw.githubusercontent.com/anthropics/skills/main/skills/skill-creator/references/workflows.md`
- **Output patterns**: `https://raw.githubusercontent.com/anthropics/skills/main/skills/skill-creator/references/output-patterns.md`

### 3. Create the Skill

Apply upstream guidance with these local defaults:

| Skill Type | Path |
|------------|------|
| User | `~/.claude/skills/<skill-name>/SKILL.md` |
| Project | `.claude/skills/<skill-name>/SKILL.md` |

### 4. Key Reminders

From upstream skill-creator (fetch for full details):
- **Concise is key**: Context window is shared; only add what Claude doesn't know
- **Progressive disclosure**: SKILL.md body < 500 lines; use `references/` for details
- **Frontmatter description**: Must include "when to use" - body loads AFTER triggering
- **No extra docs**: No README.md, CHANGELOG.md, etc.

## Existing User Skills

Reference for patterns:
```
~/.claude/skills/
├── clj-symbols/     # Clojure symbol navigation
├── code-rename/     # Symbol renaming
├── code-search/     # ripgrep patterns
├── code-symbols/    # ast-grep symbol finding
├── file-nav/        # fd file navigation
├── github/          # gh CLI operations
└── playwright/      # Browser automation
```
