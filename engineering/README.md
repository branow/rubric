# engineering

Opinionated engineering conventions, packaged as role-based skills for Claude Code.

Each skill is a role on the team. An agent acting in a role loads that role's rules.

## Skills

- **lead** — orchestrate an Agent Team: bootstrap the repo, derive the architecture, split work into milestones and tested commits, spawn role teammates, enforce conventions. User-invocable.
- **developer** — architecture, repository layout, naming, types, errors, state, persistence, APIs, dependencies, config.
- **qa** — what and how to test; E2E-first, test types, coverage.
- **security** — secrets, auth, authorization, input safety, supply chain, PII.
- **release** — version control, commits, CI/CD, deployment, observability, licensing.

Rules are firm where they target common mistakes and light where the right call is situational.
