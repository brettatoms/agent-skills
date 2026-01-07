# Agent Skills

A collection of skills for AI coding agents.

## Installation

Copy desired skill directories to your agent's skills folder (e.g., `~/.claude/skills/` for Claude Code).

## Available Skills

| Skill | Description | Key Tool |
|-------|-------------|----------|
| **[code-search](code-search/)** | Search file contents with regex patterns | ripgrep (`rg`) |
| **[file-nav](file-nav/)** | Find files and navigate directories | fd |
| **[code-symbols](code-symbols/)** | Find/edit symbols in JS, TS, Python, Go, Rust, etc. | ast-grep (`sg`) |
| **[clj-symbols](clj-symbols/)** | Find/edit Clojure symbols | clj-kondo, nREPL |
| **[code-rename](code-rename/)** | Rename symbols across a codebase | ast-grep, clj-kondo |
| **[github](github/)** | PRs, issues, Actions, releases | gh CLI |
| **[playwright](playwright/)** | Browser automation and testing | Playwright |
| **[browser-tools](browser-tools/)** | Interactive browser automation via CDP (from [pi-skills](https://github.com/badlogic/pi-skills)) | Puppeteer |
| **[lib-docs](lib-docs/)** | Fetch library documentation | Web search |
| **[skill-creator](skill-creator/)** | Create new agent skills (from [anthropics/skills](https://github.com/anthropics/skills), Claude-specific) | — |

## Skill Structure

Each skill contains:

```
skill-name/
├── SKILL.md           # Main instructions (loaded by agent)
└── references/        # Optional detailed docs
```

## Prerequisites

Most skills require external tools. Install as needed:

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
