# Memory in workflows and multi-agent systems: shared state without chaos (Chapter 32)

## Summary

- Workflows and multi-agent systems need **shared state**, but “shared memory” can become noisy or unsafe.
- Separate:
  - **workflow state** (durable, structured, checkpointed)
  - **agent long-term memory** (curated, retrievable, user-controlled)
  - **handoff payloads** (schema-first, minimal)
- Use a curator (supervisor or dedicated step) to prevent speculative memories.

## Core concepts

### Workflow state vs long-term memory

- workflow state: intermediate outputs, current node, correlation IDs
- long-term memory: preferences, project continuity

Treat as separate systems.

### Patterns

- structured state object (validated + versioned)
- workers propose memory candidates; supervisor/curator commits
- retrieval gates (don’t fetch memory every turn)
- log “why retrieved” for auditability

## Practical takeaways

- Keep workflow state structured and checkpointed.
- Keep long-term memory minimal and curated.
- Prevent speculative memory from becoming truth.
