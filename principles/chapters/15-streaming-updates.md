# Streaming updates: making long agent workflows feel alive (Chapter 15)

## Summary

- Streaming is a UX and reliability tool: users see progress instead of waiting in silence.
- Streaming fits naturally with agent/workflow execution because multi-step systems can provide:
  - partial answers
  - intermediate status messages
  - step-by-step progress events
- You can stream:
  - **tokens** (LLM output as it is generated)
  - **events** (workflow step updates, tool calls, checkpoints)
- Streaming forces you to think clearly about **state transitions** and what to surface to users.

## Core concepts

### Why streaming matters

Without streaming, users experience:

- long blank waits
- uncertainty ("is it working?")
- higher abandonment

With streaming, you can show:

- “I’m searching…”
- “I found X…”
- “I’m generating the final answer…”

### Two streaming layers

1. **Token streaming**
   - stream model output as tokens
   - good for chat UX and incremental generation

2. **Event streaming**
   - stream discrete lifecycle events:
     - step started/finished
     - tool call invoked/returned
     - checkpoint saved
   - good for debuggability and workflow UIs

### What to stream (practical guideline)

Stream what the user can understand and benefit from:

- progress + what’s happening
- partial results when stable
  Avoid leaking sensitive inner reasoning or noisy logs.

## Practical takeaways

- Use streaming for any workflow that takes more than a couple seconds.
- Prefer event streaming for complex systems, even if you also stream tokens.
- Decide upfront what event types your system emits (a small, stable schema).
