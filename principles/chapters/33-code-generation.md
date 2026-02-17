# Code generation: making agents write code you can ship (Chapter 33)

## Summary

- Reliable codegen needs **constraints, scaffolding, and verification**.
- Loop: plan → generate (small diffs) → validate (tests/lint/typecheck/build) → repair.
- Treat codegen as “assistant + CI”.

## Core concepts

- Diff-first generation reduces risk and improves reviewability.
- Verification loop is the real superpower: fix based on actual errors.
- Governance: restrict write scope and require approval for risky changes.

## Practical takeaways

- Prefer patches over rewrites.
- Always propose (or run) checks, then iterate until green.
