# Multi-agent systems 101: specialization, coordination, and failure modes (Chapter 21)

## Summary

- A **multi-agent system** is a set of agents that collaborate, often with specialization (roles) and coordination logic.
- Teams reach for multi-agent designs when:
  - tasks are naturally decomposable
  - expertise differs by subtask (research vs writing vs coding)
  - you need parallelism
- Multi-agent systems add capability, but also add:
  - coordination overhead
  - more failure modes (loops, conflicts, compounding hallucinations)
- A safe default is: start single-agent/workflow, then expand into multi-agent only when you have clear reasons.

## Core concepts

### Why multi-agent systems exist

Single agents struggle when a task requires:

- multiple distinct “modes” of thinking
- parallel work streams
- clean separation of concerns (e.g., planner vs executor)

Multi-agent systems enable:

- role specialization (researcher, coder, verifier, summarizer)
- explicit handoffs (structured messages between roles)
- parallel execution (multiple workers)

### Common multi-agent architectures

- **Supervisor + workers**
  - a top-level agent delegates to specialized agents
- **Pipeline agents**
  - each agent handles a stage (retrieve → synthesize → verify)
- **Debate/consensus**
  - multiple agents propose solutions; a judge reconciles

### Typical failure modes

- **Looping**
  - agents keep delegating back and forth without termination criteria
- **Conflict**
  - agents disagree and never converge
- **Compounding hallucinations**
  - one agent’s mistake becomes another agent’s “input truth”
- **Tool chaos**
  - multiple agents call tools redundantly or inconsistently

### Stabilizers

- strict schemas for inter-agent messages
- explicit termination criteria (“stop when X is true”)
- tool budgets and rate limits
- supervisor “judge” step for final reconciliation

## Practical takeaways

- Use multi-agent systems when specialization or parallelism is truly needed.
- Keep coordination simple and observable:
  - trace each agent’s actions and handoffs
  - enforce budgets and termination conditions
- Prefer a supervisor/worker architecture as a first multi-agent pattern.
