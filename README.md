# Agent Skills

A collection of skills for AI coding agents.

## Overview

- **Code Navigation**: Search file contents (`code-search`), find files (`file-nav`), locate symbols (`code-symbols`, `clj-symbols`)
- **Refactoring**: Rename symbols across codebases (`code-rename`)
- **Browser**: Automate browsers for testing or scraping (`playwright`, `browser-tools`)
- **GitHub**: Manage PRs, issues, Actions, releases (`github`)
- **Docs**: Fetch library documentation on demand (`lib-docs`)

### Skills

- **[code-search](code-search/)** — Search inside files with ripgrep (`rg`). Find patterns, definitions, usages, TODOs.
- **[file-nav](file-nav/)** — Find files by name and explore directories with fd.
- **[code-symbols](code-symbols/)** — Find/edit functions, classes, symbols with ast-grep (`sg`). Supports JS, TS, Python, Go, Rust, and 25+ other languages.
- **[clj-symbols](clj-symbols/)** — Find Clojure symbols with clj-kondo and nREPL.
- **[code-rename](code-rename/)** — Safely rename symbols across a codebase using ast-grep or clj-kondo.
- **[github](github/)** — Manage PRs, issues, Actions, releases with the gh CLI.
- **[playwright](playwright/)** — Browser automation via Playwright server. Navigate, click, fill forms, screenshot.
- **[browser-tools](browser-tools/)** — Chrome automation via Puppeteer/CDP. Element picker, JS eval, content extraction. (from [pi-skills](https://github.com/badlogic/pi-skills))
- **[lib-docs](lib-docs/)** — Fetch library documentation via web search.
- **[skill-creator](skill-creator/)** — Create new skills. (from [anthropics/skills](https://github.com/anthropics/skills), Claude-specific)

## Installation

Copy desired skill directories to your agent's skills folder (e.g., `~/.claude/skills/` for Claude Code).

## Skill Structure

Each skill contains:

```
skill-name/
├── SKILL.md           # Main instructions (loaded by agent)
└── references/        # Optional detailed docs
```

## Prerequisites

Most skills require external tools: `ripgrep`, `fd`, `ast-grep`, `clj-kondo`, and `gh`. Each skill's SKILL.md has detailed installation instructions for multiple platforms.

If you're on macOS and have Homebrew installed, you can install the tools with the following commands:

```bash
# Core tools
brew install ripgrep fd ast-grep

# Clojure
brew install borkdude/brew/clj-kondo

# GitHub
brew install gh

# Playwright
npm install -g playwright
```

## License

MIT
