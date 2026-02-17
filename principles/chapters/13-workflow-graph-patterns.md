# Workflow graph patterns: branching, chaining, merging, and conditions (Chapter 13)

## Summary

- Workflows become powerful when you can express **control flow** as a graph.
- Core graph patterns:
  - **Chaining**: step A → step B → step C
  - **Branching**: choose different paths based on a condition
  - **Merging**: join results from multiple branches
  - **Conditions**: narrow, testable decisions that drive the graph
- The highest reliability comes from keeping each node’s job **small and unambiguous**.

## Core patterns

### 1) Chaining

**Use when:** the output of one step is needed by the next step.

Example (conceptual):

1. Extract entities
2. Look up facts for entities
3. Write the final response with citations

**Design tip:** keep each node pure and testable: clear input → clear output.

---

### 2) Branching

**Use when:** the workflow should diverge based on a decision.

Typical branches:

- classification-based (support ticket type)
- safety-based (allowed vs disallowed)
- availability-based (data present vs missing)

**Best practice:** make branch decisions **binary** where possible:

- “Is this request about billing? yes/no”
  instead of a fuzzy multi-class label unless you truly need it.

---

### 3) Conditions

A condition is the “switch” that decides which branch runs.

**What makes a good condition**

- deterministic if possible (rule-based checks first)
- otherwise, a narrow LLM call with a strict schema:
  - return one of: `["billing", "bug", "feature", "other"]`
- heavily constrained output (later chapters: structured output + evals)

---

### 4) Merging

**Use when:** multiple branches produce partial outputs that must be combined.

Common merge shapes:

- **fan-out → fan-in**: run parallel checks, then merge into a single report
- **vote/consensus**: multiple branches propose answers, then a merge node selects or reconciles

**Design tip:** define a merge schema early, otherwise “merge” becomes “prompt soup”.

## Practical takeaways

- Think in graphs: _fan-out, fan-in, and explicit decision points_.
- Make conditions narrow and stable; everything else depends on them.
- Prefer parallelism for independent checks (latency win) and merge with a clear schema.
