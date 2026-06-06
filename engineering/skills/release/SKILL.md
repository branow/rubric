---
name: release
description: Use for version control, commits, CI/CD, deployment, environments, and operational observability. The release/ops role's rules.
user-invocable: false
---

# Role: Release / Ops

## Version control
- **Branching is situational.** Solo pet project: commit straight to `main` when sure; `main` holds changes you trust. **Experiments always on a separate branch.**
- **Prefer rebase** — clean history — in almost all cases. Merge only when someone depends on you and you can't rewrite history.
- **One PR = one meaningful change ≈ one task.** No huge PRs.

## Commits — strict
- **Conventional Commits**: `feat`/`fix` prefix + one sentence.
- **First word lowercase.**
- **No commit bodies.**
- **No `Co-Authored-By` / Claude Code attribution line — remove it.**
- Concise; say what was done and **especially why** — more why than what.

## CI/CD
- **CI is good** — testing, linting, third-party security checks all welcome. Set up per project.
- Keep projects simple, clean, and performant so **CI and build times stay cheap** — complexity makes CI expensive.

## Environments & deployment (situational)
- Real projects get multiple environments; **pet projects don't need them** (no production).
- Deployment strategy (rolling/blue-green/canary, rollback) is deferred until there's a real deployment.
- **Feature flags**: positive; sometimes env vars instead. Define per need.

## Config delivery
- **More dynamic overrides more static** — env vars override config-file values.
- **Validate config at startup and fail fast.** Config is external input.

## Observability
- Metrics and tracing matter for **bigger, network-distributed systems** — reach for **OpenTelemetry**. They exist for debugging.
- **Alerting only when there's production** or you genuinely care what happened.
- Logging: structured, minimal levels (error/warn/info/debug); duplicate key fields into both attributes and the message.

## Licensing
**Most repos open source, but decided per repository.** No general convention.
