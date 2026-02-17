# The supervisor pattern: one boss, many specialists (Chapter 22)

## Summary

- A **supervisor agent** is a coordinator that delegates to specialized worker agents, then merges results.
- This pattern reduces multi-agent chaos by centralizing control (budgets, stop criteria, final reconciliation).
- Reliability comes from:
  - narrow worker roles
  - structured handoffs (schemas)
  - explicit termination criteria
  - a “merge + verify” final step

## Core concepts

### What a supervisor does

1. read the request
2. decompose into subtasks
3. select workers
4. send structured briefs (task + constraints + output schema)
5. gather outputs
6. reconcile into the final answer

### Worker design checklist

For each worker define:

- purpose + done criteria
- allowed tools (least privilege)
- required output schema
- constraints (citations, no speculation, etc.)

### Supervisor design checklist

- limit delegation (max workers, max steps)
- enforce budgets (tool calls, tokens, time)
- require evidence fields where relevant
- run a reconciliation step that resolves conflicts

## Practical takeaways

- Start multi-agent with **supervisor + workers**.
- Use strict schemas for worker outputs.
- Make “stop conditions” explicit to avoid loops.
