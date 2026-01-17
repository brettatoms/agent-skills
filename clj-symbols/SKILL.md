---
name: clj-symbols
description: Find and edit Clojure symbols using clj-kondo and REPL introspection. Use when finding Clojure function definitions, var usages, namespace contents, renaming symbols, or replacing function bodies.
allowed-tools: Bash, Read, Edit, Task
---

# Clojure Symbols Skill

Use **clj-kondo** for static analysis and **clj-nrepl-eval** for REPL introspection to find Clojure symbols.

For non-Clojure languages, use the `code-symbols` skill instead.

## Prerequisites

### clj-kondo

```bash
# Arch Linux
sudo pacman -S clj-kondo

# Homebrew
brew install borkdude/brew/clj-kondo

# Script
curl -sLO https://raw.githubusercontent.com/clj-kondo/clj-kondo/master/script/install-clj-kondo
chmod +x install-clj-kondo && ./install-clj-kondo
```

### clj-nrepl-eval

Discover running nREPL servers:
```bash
clj-nrepl-eval --discover-ports
```

## Quick Reference

| Task | Tool | Command |
|------|------|---------|
| List symbols in file | clj-kondo | `clj-kondo --lint file.clj --config '{...}'` |
| Find definition | clj-kondo | Filter `var_definitions` by name |
| Find usages | clj-kondo | Filter `var_usages` by name |
| Get var metadata | REPL | `(meta #'ns/var)` |
| Get source | REPL | `(clojure.repl/source fn)` |

## Core clj-kondo Commands

### Find Symbol Definition

```bash
clj-kondo --lint src components bases \
  --config '{:output {:format :json}, :analysis {:var-definitions true}}' \
  | jq '(.analysis.var_definitions // [])[] | select(.name == "my-function")'
```

### Find Symbol Usages

```bash
clj-kondo --lint src components bases \
  --config '{:output {:format :json}, :analysis {:var-usages true}}' \
  | jq '(.analysis.var_usages // [])[] | select(.name == "my-function")'
```

### List Symbols in File

```bash
clj-kondo --lint path/to/file.clj \
  --config '{:output {:format :json}, :analysis {:var-definitions true}}' \
  | jq '.analysis.var_definitions // []'
```

### Output Fields

**var_definitions:**
- `filename`, `row`, `col`, `end_row`, `end_col` - Location
- `ns`, `name` - Identity
- `defined_by` - Defining form (e.g., `clojure.core/defn`)
- `fixed_arities`, `doc`, `private`

**var_usages:**
- `from`, `to` - Source and target namespaces
- `name`, `arity` - Symbol name and call arity
- `from_var` - Containing function
- `row`, `col` - Location

## Core REPL Commands

```bash
# Get var metadata
clj-nrepl-eval -p PORT "(meta #'my.namespace/my-function)"

# Get source code
clj-nrepl-eval -p PORT "(clojure.repl/source my-function)"

# List namespace contents
clj-nrepl-eval -p PORT "(dir my.namespace)"

# Search by pattern
clj-nrepl-eval -p PORT "(clojure.repl/apropos \"pattern\")"
```

## Common Workflow

### Find Definition and All Usages

```bash
# Find definition
clj-kondo --lint . \
  --config '{:output {:format :json}, :analysis {:var-definitions true}}' \
  | jq '(.analysis.var_definitions // [])[] | select(.name == "target-fn") | {ns, name, filename, row}'

# Find all usages
clj-kondo --lint . \
  --config '{:output {:format :json}, :analysis {:var-usages true}}' \
  | jq '(.analysis.var_usages // [])[] | select(.name == "target-fn") | {from, from_var, filename, row}'
```

## Additional References

- **clj-kondo details**: See [references/kondo.md](references/kondo.md) for full analysis options
- **REPL introspection**: See [references/repl.md](references/repl.md) for REPL commands
- **Editing/renaming**: See [references/editing.md](references/editing.md) for rename workflows
