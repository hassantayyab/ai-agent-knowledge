# Workflows 101: when agents have too much freedom (Chapter 12)

## Summary

- Agents can call _any_ tool at _any_ step, which is powerful but sometimes **too unconstrained** for predictable systems.
- **Graph-based workflows** are useful when you need **more determinism** than an agent provides.
- When reliability matters, you often need to **break problems down** into smaller steps and **binary decisions** rather than one big “do everything” decision.
- A workflow primitive helps with:
  - **branching logic**
  - **parallel execution**
  - **checkpoints**
  - **tracing/observability**

## Core concepts

### Agents vs workflows

- **Agent:** flexible, adaptive, can decide which tools to call and in what order.
- **Workflow:** more like a _structured program_ or _decision tree_ where you define the graph and the model fills in narrower decisions.

### Why workflows exist

Workflows appear when:

- your agent’s behavior is inconsistent
- you need repeatability and control
- the problem can be decomposed into steps (and often should be)

### The “binary decisions” pattern

Instead of one complex prompt like:

- “Read this input and do everything end-to-end”

You design:

- multiple small calls
- each making a **narrow decision** (yes/no, classification, extraction)
- connected by branching/merging logic

### What workflow primitives provide

A workflow engine/primitive typically supports:

- **Branching:** choose different paths based on outcomes
- **Parallelism:** run independent checks at once
- **Checkpoints:** pause/resume long processes safely
- **Tracing:** see what happened across the graph

## Practical takeaways

- Reach for workflows when you need **predictability** more than flexibility.
- Keep steps small and testable.
- Add tracing early so you can debug graph behavior in production.
