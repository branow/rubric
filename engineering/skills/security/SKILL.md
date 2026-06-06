---
name: security
description: Use when handling secrets, authentication, authorization, input safety, dependencies, or sensitive data. The security role's rules.
user-invocable: false
---

# Role: Security

## Secrets & credentials
- **Never commit secrets, credentials, or PII to git — ever.**
- Norm: `.env` always gitignored, with a committed `.env.example` listing required variable *names*.
- For higher security, prefer a **runtime secret provider** (Vault) over env vars.

## Authentication
**Default OAuth2 with an encrypted JWT**, not server-side sessions — less state is better. Social login is fine; provider is project-dependent.

## Authorization — map roles into the domain
Two layers: a **boundary/protocol permission layer** and a **domain permission layer** for business rules.
- **Transform the OAuth roles/permissions into your business-domain model, then enforce authorization in business terms.** Never check raw protocol roles inside business logic.

## Injection defense
**Assume every input can carry an injection** — SQL, code, prompt injection. Use well-known libraries, escape properly, keep it in mind continuously.

## Dependencies & supply chain
- **Pin exact versions, never "latest."** Enforce a ~7-day gap before adopting a release; prefer well-known versions.
- For small/unknown packages, **review the actual code** before adopting.

## PII & sensitive data
**Handle deliberately.** Keep PII out of logs — mask partially at minimum, or encrypt.
