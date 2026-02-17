# Agent middleware: the safety and reliability layer outside the model (Chapter 9)

## Summary

- Middleware is the non-LLM code that wraps agents to enforce:
  - permissions
  - rate limits and budgets
  - retries and fallbacks
  - logging/tracing
  - validation and redaction
- The model should not be your only line of defense. Put critical checks in middleware.

## Core concepts

### What middleware does

- **Pre-checks**: auth, tenant, policy gates
- **Routing**: select model and mode
- **Tool governance**: allowlists, arg validation, dry-run, approvals
- **Post-processing**: schema validation, redaction, formatting
- **Observability**: traces, metrics, cost accounting

### Middleware pipeline pattern

Think “HTTP middleware”, but for agent steps.

## Code example: middleware around tool calls

```python
def call_tool(tool_name, args, ctx):
    assert tool_name in ctx.allowed_tools

    args = validate_args(tool_name, args)

    with trace_span("tool_call", tool=tool_name):
        result = TOOL_REGISTRY[tool_name](**args)

    result = redact_secrets(result)
    return result
```

### Where to enforce ACLs

- Enforce access in middleware (DB query filters, doc permissions).
- Do NOT rely on the model to “only use allowed docs”.

## Practical takeaways

- Middleware is where reliability lives: budgets, retries, permissions, validation.
- Keep the model focused on reasoning; keep enforcement in code.
- Every agent action should be traceable and auditable.
