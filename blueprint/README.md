# blueprint

Co-author design specs with Claude — concise, decision-dense, built one chunk at a time.

A blueprint reads like a language spec, not prose. You split a system into abstractions (CLI commands, endpoints, processes, components), then pin down each one's exact contract: what it does, what state it owns, where that lives, in what format, and why this way and not another. Every line is load-bearing; the why rides inline with the decision.

The skill is a **method**, not a template. It drives the work interactively: frame the system, agree the decomposition, then go chunk by chunk — draft, interrogate for gaps, challenge, decide, lock. Claude acts as a thinking partner, not a stenographer.

## Skill

- **blueprint** — `/blueprint`. User-invocable. Ships with a real four-file reference blueprint for calibration.

## Install

```
/plugin marketplace add branow/rubric
/plugin install blueprint@rubric
```

## License

[MIT](../LICENSE)
