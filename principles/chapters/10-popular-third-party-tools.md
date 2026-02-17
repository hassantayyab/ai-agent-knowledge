# Popular third-party tools: the ecosystem map (Chapter 10)

## Summary

- The agent ecosystem is a toolbox, not a religion.
- Most tools fall into a few categories:
  - orchestration/workflows
  - retrieval/RAG frameworks
  - vector databases
  - observability/tracing
  - evals/testing
  - model gateways
- Choose tools based on operational fit and team ergonomics, not hype.

## Core categories (what they’re for)

### Orchestration & workflows

Use when you need:

- step graphs
- retries/checkpoints
- deterministic routing/branching

Typical outputs:

- run logs and step traces

### RAG & indexing frameworks

Use when you need:

- chunking pipelines
- embedding/index integration
- retrieval helpers

Still evaluate retrieval quality yourself (gold sets + recall@k).

### Vector databases / storage

Use when you need:

- similarity search at scale
- filtering/metadata constraints
- multi-tenant ACL support

### Observability & tracing

Use when you need:

- traces per run/tool call
- latency + cost monitoring
- debugging reproducibility

### Evals & testing

Use when you need:

- regression gates for prompts/tools/retrieval
- LLM-judge scoring with rubrics
- harnesses for mocked tools

### Model gateways

Use when you need:

- unified API for multiple providers/models
- routing policies, fallback, caching
- auditing and rate limiting

## Practical takeaways

- Prefer tools that integrate cleanly with your **middleware** layer (Ch 9).
- Avoid lock-in by keeping a retrieval/tool interface in your own code.
- “Frameworks help you move fast” but your evals/traces decide what’s correct.
