---
name: lib-docs
description: Fetch library documentation and code examples. Use when user asks about library APIs, needs code examples, or says "use lib-docs", "lookup docs for X", "how does [library] work". Works with any language/framework.
allowed-tools: WebSearch, WebFetch, Read
---

# Library Documentation Skill

Fetch up-to-date documentation for any library using web search and pre-configured sources.

## Quick Reference (Common Sources)

| Library | Primary Source |
|---------|----------------|
| Clojure libs | `cljdoc.org/d/{group}/{artifact}` |
| HTMX | `htmx.org/docs/` |
| Alpine.js | `alpinejs.dev/` |
| Tailwind | `tailwindcss.com/docs/` |
| DaisyUI | `daisyui.com/components/` |
| React | `react.dev/reference/` |

For full list: See [references/sources.md](references/sources.md)

## Workflow

### 1. Resolve Documentation Source

Check if library has a pre-configured source in `references/sources.md`:
- **Known library**: Use configured URL directly
- **Unknown library**: WebSearch for `"[library name] official documentation"`

### 2. Fetch Documentation

Use WebFetch with a focused prompt:
```
WebFetch URL with prompt: "Extract [topic] documentation, API reference, and code examples for [library]"
```

**Tips for effective fetches:**
- Target specific pages (API reference, getting started) over landing pages
- Include version if user specified one
- Request code examples explicitly

### 3. Summarize for User

Return:
- Relevant API/function signatures
- Code examples demonstrating usage
- Link to source for further reading

## Fallback: Unknown Libraries

When no pre-configured source exists:

1. **WebSearch**: `"[library] official documentation site"`
2. **Identify official source** from results (prefer: official site > GitHub > Stack Overflow)
3. **WebFetch** the docs page
4. **Offer to add** the source to `references/sources.md` for future lookups

## Examples

**User**: "How do I use HoneySQL's where clause?"
1. Check sources.md → HoneySQL on cljdoc.org
2. WebFetch `https://cljdoc.org/d/com.github.seancorfield/honeysql/` with prompt about where clauses
3. Return examples and API info

**User**: "lookup docs for some-obscure-lib"
1. Check sources.md → not found
2. WebSearch for official docs
3. WebFetch from search result
4. Return docs + offer to save source

## Token Efficiency

- Only fetch what's needed for the specific question
- Prefer targeted page URLs over index pages
- Use focused prompts to extract relevant sections
