# Tool calling: letting agents safely touch the world (Chapter 6)

## Summary

- Tool calling turns an LLM into an **actor**: it can fetch facts, run code, and mutate systems.
- Reliability requires:
  - clear tool specs
  - schema-validated arguments
  - safe retries and error handling
  - least privilege + allowlists
- Treat tool results as data; treat tool instructions inside data as untrusted.

## Core concepts

### When to call a tool

Call tools when the answer depends on:

- fresh/real-time data (prices, status, availability)
- private systems (DB, ticketing, CRM)
- deterministic computation
- file operations/code execution

### Tool spec (what the model needs)

Each tool should define:

- name
- description (when to use)
- input schema
- output shape (or examples)
- failure modes (timeouts, empty results)

### Safety patterns

- **Allowlist tools** per agent role
- **Validate args** strictly
- **Budget tool calls** (max N, stop conditions)
- **Human approval** for high-risk actions
- **Middleware enforcement** (permissions outside the model)

## Code example: tool wrapper with validation

```python
from pydantic import BaseModel, ValidationError

class SearchArgs(BaseModel):
    query: str
    top_k: int = 5

def tool_search(args: dict):
    parsed = SearchArgs(**args)  # raises ValidationError if wrong
    return search_backend(parsed.query, parsed.top_k)

TOOLS = {"search": tool_search}
```

## Code example: tool-call execution loop (pseudo)

```python
decision = model_decision(messages, tool_schemas)

if decision.type == "tool_call":
    tool_name = decision.tool.name
    args = decision.tool.args

    try:
        result = TOOLS[tool_name](args)
    except ValidationError as e:
        # repair: ask model to fix args based on schema errors
        messages.append({"role": "system", "content": f"Fix tool args error: {e}"})
    except TimeoutError:
        # retry or fall back
        ...
    else:
        messages.append({"role": "tool", "name": tool_name, "content": str(result)})
```

## Practical takeaways

- Tools are power. Wrap them with validation + permissions.
- Most “tool calling failures” are actually **bad args** or **bad result interpretation**.
- Log every tool call and its outcome for debugging and evals.
