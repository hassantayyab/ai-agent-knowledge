# Observability and tracing: debugging agents like distributed systems (Chapter 16)

## Summary

- Agents + workflows behave like **distributed systems**: multiple steps, tool calls, retries, and branches.
- Without observability, you’re blind. Tracing is how you answer:
  - “What did the model see?”
  - “Which tools were called, with what inputs?”
  - “Where did it fail, and why?”
- A practical observability stack includes:
  - **traces** (end-to-end execution)
  - **spans** (step/tool/model calls)
  - **structured logs** (inputs/outputs, metadata)
  - **metrics** (latency, error rates, cost)
- Tracing enables targeted iteration: fix prompt/tool/memory issues based on evidence rather than guesswork.

## Core concepts

### Why agents require tracing

A single LLM call is inspectable. An agent run is often:

- multiple model calls
- tool calls
- branching decisions
- memory retrieval
- retries and fallbacks

That complexity makes “it gave a bad answer” insufficient. You need _the execution record_.

### Traces and spans (mental model)

- **Trace:** the whole request lifecycle (user request → final response)
- **Span:** a sub-step in the trace:
  - LLM call span
  - tool execution span
  - memory retrieval span
  - workflow node span

### What you should record

At minimum:

- user input (sanitized)
- system prompt/instructions version
- tool calls (name, arguments, results)
- model used + parameters
- retrieved memory/context snippets (sanitized)
- errors, retries, fallbacks
- latency and token/cost estimates

### Why observability improves product quality

With traces, you can:

- find prompt injection success paths
- detect tool misuse patterns
- see which memory recalls are irrelevant
- tune routing rules and retry policies
- build eval datasets from real failures

## Practical takeaways

- Treat tracing as “non-negotiable infra” once you ship agents.
- Instrument every major step (LLM calls, tools, retrieval, branching).
- Use traces to drive iteration: fix the most common failure modes first.
