# Evaluating tool calling: selection, args, robustness, interpretation (Chapter 27)

## Summary

- Tool-use evals test:
  - **selection** (call vs no-call; which tool)
  - **arguments** (schema-valid)
  - **robustness** (errors/timeouts/empties)
  - **interpretation** (uses results faithfully)
- Use traces to create datasets and debug failures.

## Core concepts

### What to test

1. Tool selection
2. Argument construction (schema validation)
3. Retry/fallback behavior
4. Result interpretation
5. Multi-step tool plans (ordering + stop conditions)

### Deterministic eval runs

Mock tools:

- fixed outputs
- controlled errors (timeout/500/empty)

## Practical takeaways

- Split evals by failure source (selection vs args vs interpretation).
- Mock tools for repeatability.
- Use trace mining to build the dataset.
