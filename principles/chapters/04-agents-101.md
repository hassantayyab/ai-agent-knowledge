# Agents 101: what an agent is and how it differs from chat (Chapter 4)

## Summary

- An **agent** is an LLM system that can **plan + act** over multiple steps, often using **tools**, **state**, and **feedback loops**.
- The key difference from a single prompt: agents have a **control loop** (decide → act → observe → decide).
- Good agents are built from simple parts:
  - a goal (task)
  - a policy (instructions/constraints)
  - tools (APIs, code execution)
  - memory/state (what persists)
  - evaluation + observability (how you measure/debug)

## Core concepts

### What makes something an “agent”

An agent typically has:

- **Autonomy**: it chooses next actions (tool calls, follow-ups).
- **Iteration**: it can take multiple steps until completion.
- **Grounding**: it uses tools/data instead of guessing.
- **Stopping criteria**: it knows when to stop.

### Agent loop (mental model)

1. **Interpret** the user goal
2. **Plan** next step(s)
3. **Act** (call a tool / write output)
4. **Observe** results
5. **Update state** (memory/workflow state)
6. Repeat until **done** or **blocked**

### “Workflow” vs “agent”

- **Workflow**: mostly deterministic steps, good for reliability.
- **Agent**: flexible decisions, good for open-ended tasks.
  Best practice: **start workflow-like**, then add autonomy only where it helps.

## Minimal code example: a tiny agent loop (Python)

```python
def run_agent(user_request, tools, max_steps=6):
    state = {"messages": [{"role": "user", "content": user_request}]}

    for step in range(max_steps):
        # 1) Ask model what to do next (could return tool calls)
        decision = model_decide_next_action(state["messages"], tools)

        if decision["type"] == "final":
            return decision["answer"]

        if decision["type"] == "tool_call":
            tool_name = decision["tool"]["name"]
            tool_args = decision["tool"]["args"]

            # 2) Execute tool
            tool_result = tools[tool_name](**tool_args)

            # 3) Feed result back in
            state["messages"].append({"role": "tool", "name": tool_name, "content": str(tool_result)})

    return "Stopped: max_steps reached (need clarification or more budget)."
```

## Practical takeaways

- Define **done** clearly (the #1 agent reliability lever).
- Prefer **small tool calls** and short loops.
- Add tracing/evals early so you can debug when it inevitably gets weird.
