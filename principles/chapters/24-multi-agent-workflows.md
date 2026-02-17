# Multi-agent workflows: combining specialization with graph control (Chapter 24)

## Summary

- Combine multi-agent with workflows: a graph controls **when** agents run and **how** outputs flow.
- Workflows add determinism:
  - ordering
  - branching rules
  - checkpoints
  - merge semantics
- Common pattern: **workflow = orchestrator**, **agents = workers**.

## Core concepts

### Typical graph shapes

- **Pipeline**: Research → Draft → Verify → Publish
- **Fan-out/fan-in**: specialists in parallel → merge report
- **Conditional escalation**: if risk detected → run expert reviewer

### Standardize these

- handoff schemas (Ch 23)
- merge schema for combined outputs
- budgets (max steps/tool calls)
- trace/correlation IDs for observability

## Practical takeaways

- Use workflows to control multi-agent behavior when reliability matters.
- Keep agents focused; let the workflow handle control flow.
