# Dynamic agents: adapting prompts, tools, and plans at runtime (Chapter 8)

## Summary

- Dynamic agents change behavior at runtime: different tools, prompts, or strategies depending on context.
- This can improve capability, but increases unpredictability.
- Make dynamics explicit:
  - route by intent
  - constrain toolsets per mode
  - require structured outputs for decisions
  - log every mode switch

## Core concepts

### What “dynamic” means

At runtime, the agent may:

- choose a different **prompt template**
- enable/disable tool subsets
- change budgets (steps/tool calls)
- pick different models
- switch to a workflow (more deterministic)

### Common dynamic patterns

- **Intent modes**: `support`, `coding`, `research`, `summarize`
- **Escalation**: start cheap, escalate when confidence is low
- **Capability gating**: only allow risky tools after explicit user confirmation

### Risks

- mode-switching errors (wrong tools)
- inconsistent outputs
- higher injection surface if browsing/RAG is on

## Code example: mode switch with tool gating

```python
MODES = {
  "coding": {"tools": ["repo_search", "run_tests"], "max_steps": 8},
  "research": {"tools": ["web_search", "summarize"], "max_steps": 6},
  "support": {"tools": [], "max_steps": 4},
}

mode = classify_intent(user_text)  # returns one of the keys
policy = MODES[mode]
allowed_tools = policy["tools"]
```

## Practical takeaways

- Dynamic behavior must be observable (trace mode + toolset + budgets).
- Keep routing decisions structured (enums + validation).
- Prefer “dynamic outside the model” (router/middleware) over hidden prompt tricks.
