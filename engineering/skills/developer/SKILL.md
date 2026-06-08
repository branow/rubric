---
name: developer
description: Use for any task that writes, modifies, refactors, or structures code in any language — how to shape architecture and layering, repo/file layout, naming, types, errors, state, persistence, APIs, dependencies, and config. The developer role's rules.
user-invocable: false
---

# Role: Developer

Write code that reduces the reader's cognitive load. Everything below serves that.
Where a rule says "situational," use judgment. Where it's stated flatly, it's non-negotiable.

## Architecture — derive layers from the task
Before coding, decompose the whole task into a **tree of abstraction layers**. Find the concepts, give each a layer, expose one concept per layer and hide the rest. The reader has one entry point and walks down to detail.

- Shape the tree for humans: a layer can have 20 peer children (e.g. 20 tool adapters) or 3 — there is **no "2–5 children" rule.** Too deep (every node has one child) is as bad as too flat.
- Dependencies flow **bottom → top**: a parent depends on its children; **siblings stay independent.** Domain never imports infra.
- Cross-cutting concerns (config, errors, logging) are handled explicitly across layers, not sprinkled.
- Utilities **emerge from observed duplication** — don't invent them upfront.
- A boundary smell: a layer doing two jobs, detail leaking up a level, or duplication signalling a missing abstraction.

## Repository structure
- **Repo boundary = deployment boundary.** One deployment unit → one repo. Independent lifecycle → separate repo.
- **Abstract root, no detail leaking up.** Root holds repo-level files only: README, LICENSE, top folders (`src`, `docs`, `tests`), and a single root config file only when convention demands it (`tsconfig.json`, `go.mod`). Config that is itself code goes in a folder under `src` — never a pile of loose config at root.
- **Follow the technology's enforced layout** when it has one (Java, Go); otherwise apply these defaults.
- README: concise, medium tone, **no emoji**. Purpose → usage → licensing/references.

## Naming & style
- Descriptive **within context**, full words, **prefer single-word** names, **snake_case** unless the tech has its own convention.
- **Formatter + linter from day one** — opinionated, auto-applied. No diffs caused by formatting. (Go's enforced formatter is the model.)
- Line length ~80, max 100. Keep imports ordered. No custom file headers.

## Types & data
- **Strict types always.** In untyped languages (JS) use TypeScript and keep everything typed; never leak `any`. Type Python too.
- **Cross-kind conversions must be explicit and visible** (`string` → `number` written out). Ban silent coercion. Same-family widening (`int` → `long`) is low-risk.
- `undefined` = never existed (don't assign it). `null` = a legitimate "missing" state. Mark optionals with the language's own mechanism.
- **Validate external input at the boundary** with a schema lib (Zod for TS). Transform to clean types there, then trust them downstream. Skip only when the protocol guarantees types (gRPC).

## Trust between modules
Within one repo, **modules trust each other** — if module A already checked something, B does not re-check it. Trust the code you wrote; distrust external input. No defensive re-validation inside the system.

## Errors
An **error is structured information** (codes, attributes, fields) — not a user-facing string. Never bake context into a message (`"cannot parse <path>"` is the smell — put the path on an attribute). Business logic throws; the **boundary** reads the error's type/attributes and maps it to output.
- **Throw = unexpected / input you don't intend to handle.** Expected branches are states (discriminated unions), not throw/catch. Throw/catch as control flow is an architecture smell.

## State & persistence
- Give state an owner named after its concept; prefer immutable-by-default.
- **Start from the data you actually need — don't reach for a database when a file will do.** SQL → Postgres; local → SQLite; pick graph/vector/document by shape; queues/cache by need.
- **Control the SQL.** Dislike ORM-as-magic — prefer query building or raw SQL where you own the query and know the result shape; object mapping can sit on top, but the SQL-sending layer stays yours.
- Always plan **schema migrations** when there's a database (reversible, zero-downtime where possible).
- **Cache only at a real bottleneck.** If you cache you must invalidate — prefer time-based; design real invalidation when changes affect UX.

## APIs (situational)
REST/RPC/GraphQL by situation — no default. **No backward compatibility until something depends on you.** Always keep a recorded version (git + in-project). Share contracts via the layer's mechanism (OpenAPI/protobuf/types package). Always provide pagination, filtering, and a defined error shape.

## Concurrency & performance
- **Don't use concurrency unless it saves seconds** — not a few hundred ms.
- **Optimize for user experience first** — when slowness is felt. No formal budgets, but keep programs **lean** (the 2MB stdio service is the ideal, not the 2GB-RAM idle app).

## Dependencies
- Pick a library **focused on your exact problem**; if it's bloated with unrelated concerns, copy the one piece you need instead. Prefer robust, well-known, maintained.
- **Pin exact versions, never "latest"**; enforce a ~7-day-old-version gap; commit lockfiles always.
- **Think past the TS+npm / Python+pip reflex.** Use a good package manager (UV for Python, pnpm over npm). Builds must be simple and quick — prefer a tech that ships its own builder.

## Config & logging
- **Validate config at startup and fail fast** — config is external input. More dynamic overrides more static (env vars override config values).
- **Log at boundaries, not in business logic.** Structured logs, minimal levels (error/warn/info/debug). Duplicate key fields into both attributes (for querying) and the message (for reading).

## Documentation
- Doc-comment every public function: what (one line), why, non-obvious params. Comments explain **why**, never restate code. No commented-out code, no committed TODOs.
- Keep **general MD files in `docs`** describing high-level architecture/decisions — the entry point before per-function specs. Generate changelog from git logs; don't maintain a separate one.
