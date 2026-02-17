# Model routing and structured output: choosing brains and formats (Chapter 5)

## Summary

- **Model routing** chooses _which model_ handles a request (cheap/fast vs smart/expensive).
- **Structured output** forces predictable machine-readable responses (usually JSON) to avoid brittle parsing.
- Together they enable:
  - cost/latency control
  - reliability in workflows
  - easier evals and automation

## Core concepts

### Model routing

Routing is a policy that maps requests to models based on:

- complexity (simple Q&A vs multi-step reasoning)
- modality (text vs image)
- latency/cost constraints
- safety requirements
- tool use needs

**Common routing patterns**

- **Tiered**: small model first, escalate if needed.
- **Specialized**: different models per task type (code, vision, extraction).
- **Fallback**: retry with a stronger model when the weaker one fails.

### Structured output

Use schemas when downstream code needs:

- stable fields
- enums for routing decisions
- validated arguments for tool calls

**Pattern**

1. Ask for JSON matching a schema
2. Parse + validate
3. Repair/retry on validation errors

## Code example: routing + structured output (TypeScript-like)

```ts
type Intent = 'qa' | 'code' | 'rag' | 'tool_task';

function routeIntent(userText: string): Intent {
  // lightweight classifier or rules
  if (userText.includes('error') || userText.includes('stacktrace')) return 'code';
  if (userText.includes('search') || userText.includes('docs')) return 'rag';
  if (userText.includes('call') || userText.includes('api')) return 'tool_task';
  return 'qa';
}

function selectModel(intent: Intent) {
  switch (intent) {
    case 'qa':
      return 'small-fast';
    case 'code':
      return 'strong-code';
    case 'rag':
      return 'balanced';
    case 'tool_task':
      return 'strong-reliable';
  }
}
```

## Code example: schema validation + repair loop (Python)

```python
import json
from jsonschema import validate, ValidationError

SCHEMA = {
  "type": "object",
  "properties": {
    "intent": {"enum": ["qa", "code", "rag", "tool_task"]},
    "needs_tools": {"type": "boolean"},
    "confidence": {"type": "number", "minimum": 0, "maximum": 1},
  },
  "required": ["intent", "needs_tools", "confidence"]
}

def parse_or_repair(model_output: str, repair_fn):
    for _ in range(3):
        try:
            obj = json.loads(model_output)
            validate(instance=obj, schema=SCHEMA)
            return obj
        except (json.JSONDecodeError, ValidationError) as e:
            model_output = repair_fn(model_output, str(e))
    raise RuntimeError("Could not produce valid structured output")
```

## Practical takeaways

- Route early to control cost and keep latency predictable.
- Use structured outputs whenever a decision triggers automation or branching.
- Always validate. Treat “almost valid JSON” as invalid.
