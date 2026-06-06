---
name: qa
description: Use when deciding what and how to test — choosing test types, writing tests, and judging coverage. The QA role's rules.
user-invocable: false
---

# Role: QA

Test for the value a test provides, never to hit a number. A test that needs heavy mocking to isolate one `if` tests nothing.

## Default posture: E2E-first
- **Lean toward end-to-end tests, and almost always integration tests.** E2E exercises the full tool.
- **Design for a wide testing surface.** A CLI is many commands hitting different parts of the system — that breadth makes E2E easy and modular. Some projects get **no unit tests at all**, fully covered by E2E.

## What each test type is for
- **Unit** → interesting/complex logic inside one algorithm, where you need to pin down behavior. *Not* for binding glue.
- **Integration** → real external dependencies and your communication with them. Stand up a real instance (a real Bitbucket server found many real bugs). Slow by design — fewer, make them count.
- **Component** → a genuinely complex component. Rare.
- **E2E** → the whole system from outside; the main flows, not edge cases.

## What NOT to test
- **Binding/glue between modules** — that's proven by E2E or integration, not unit tests. Unit-testing glue is pointless.
- Anything volatile or early-stage gets the main success path only, not exhaustive boundary tests that you'll rewrite.

## Coverage
**Don't track a coverage number.** If it's cheap and simple for the tech, fine to add — but it's not the goal. Focus on the real things worth testing.

## Performance / load testing
Do it when you expect **heavy computation or heavy I/O**, to validate it holds up. Skip it for simple work that will never see big data volumes.
