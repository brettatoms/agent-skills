# Library Documentation Sources

Pre-configured documentation URLs for common libraries. Add new libraries as needed.

## Table of Contents
- [Clojure Ecosystem](#clojure-ecosystem)
- [Web/Frontend](#webfrontend)
- [JavaScript/TypeScript](#javascripttypescript)
- [CSS Frameworks](#css-frameworks)
- [Databases](#databases)

---

## Clojure Ecosystem

### General Pattern
Most Clojure libraries: `https://cljdoc.org/d/{group}/{artifact}/`

### Ring
- **Docs**: https://github.com/ring-clojure/ring/wiki
- **API**: https://cljdoc.org/d/ring/ring-core/

### Compojure
- **Docs**: https://github.com/weavejester/compojure/wiki
- **API**: https://cljdoc.org/d/compojure/compojure/

### HoneySQL
- **Docs**: https://cljdoc.org/d/com.github.seancorfield/honeysql/
- **Getting Started**: https://github.com/seancorfield/honeysql#getting-started

### next.jdbc
- **Docs**: https://cljdoc.org/d/com.github.seancorfield/next.jdbc/
- **Getting Started**: https://github.com/seancorfield/next-jdbc/blob/develop/doc/getting-started.md

### Integrant
- **Docs**: https://github.com/weavejester/integrant
- **API**: https://cljdoc.org/d/integrant/integrant/

### Malli
- **Docs**: https://github.com/metosin/malli
- **API**: https://cljdoc.org/d/metosin/malli/

### Reitit
- **Docs**: https://cljdoc.org/d/metosin/reitit/
- **Introduction**: https://github.com/metosin/reitit#introduction

### Hiccup
- **Docs**: https://github.com/weavejester/hiccup
- **API**: https://cljdoc.org/d/hiccup/hiccup/

### core.async
- **Docs**: https://clojure.github.io/core.async/
- **Guide**: https://clojure.org/guides/async_walkthrough

### spec / spec.alpha
- **Guide**: https://clojure.org/guides/spec
- **API**: https://clojure.github.io/spec.alpha/

---

## Web/Frontend

### HTMX
- **Docs**: https://htmx.org/docs/
- **Reference**: https://htmx.org/reference/
- **Examples**: https://htmx.org/examples/

### Alpine.js
- **Docs**: https://alpinejs.dev/start-here
- **Directives**: https://alpinejs.dev/directives/
- **Magics**: https://alpinejs.dev/magics/

### Hyperscript
- **Docs**: https://hyperscript.org/docs/
- **Reference**: https://hyperscript.org/reference/

---

## JavaScript/TypeScript

### React
- **Docs**: https://react.dev/
- **Reference**: https://react.dev/reference/react
- **Hooks**: https://react.dev/reference/react/hooks

### Next.js
- **Docs**: https://nextjs.org/docs
- **API Reference**: https://nextjs.org/docs/app/api-reference

### Node.js
- **Docs**: https://nodejs.org/docs/latest/api/
- **Guides**: https://nodejs.org/en/learn

### TypeScript
- **Handbook**: https://www.typescriptlang.org/docs/handbook/
- **Reference**: https://www.typescriptlang.org/docs/handbook/utility-types.html

---

## CSS Frameworks

### Tailwind CSS
- **Docs**: https://tailwindcss.com/docs/
- **Reference**: https://tailwindcss.com/docs/installation

### DaisyUI
- **Components**: https://daisyui.com/components/
- **Config**: https://daisyui.com/docs/config/

---

## Databases

### SQLite
- **Docs**: https://www.sqlite.org/docs.html
- **SQL Reference**: https://www.sqlite.org/lang.html

### PostgreSQL
- **Docs**: https://www.postgresql.org/docs/current/
- **SQL Reference**: https://www.postgresql.org/docs/current/sql.html

---

## Adding New Libraries

To add a new library, append an entry with:
```markdown
### Library Name
- **Docs**: https://...
- **API**: https://...
- **Examples**: https://... (optional)
```
