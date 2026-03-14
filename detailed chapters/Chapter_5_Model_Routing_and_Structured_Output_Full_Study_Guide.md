# Chapter 5 — Model Routing and Structured Output

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 5** clearly and memorably using diagrams, tables, examples, code, and practical engineering intuition.

---

## 1) Summary of the chapter

Chapter 5 teaches two foundational engineering ideas for production AI systems:

1. **Model routing**: your application should be able to switch between models and providers without rewriting all your logic.
2. **Structured output**: your application should often ask the model to return data in a defined schema instead of loose prose.

This chapter is really about moving from:

```text
"just chatting with a model"
```

to:

```text
"building an application around a model"
```

That is the real step forward.

### The whole chapter in one line

```text
Routing chooses the brain. Structured output chooses the shape.
```

---

## 2) The chapter in one figure

```text
                 User / App request
                         │
                         ▼
                ┌──────────────────┐
                │  Model routing   │
                │ choose provider  │
                │ choose model      │
                └──────────────────┘
                         │
                         ▼
                ┌──────────────────┐
                │   LLM response   │
                │ but shaped by    │
                │ structured schema│
                └──────────────────┘
                         │
                         ▼
                 Clean app-usable output
```

---

## 3) Why this chapter matters

A beginner often uses an LLM like this:

```text
prompt → blob of text → manually read it
```

But real applications need something closer to this:

```text
request → choose best model → get predictable output → pass into code/UI/workflow
```

That is exactly what this chapter is introducing.

### The hidden shift

This chapter marks the transition from:

- prompt experimentation

to:

- software architecture around LLMs

That makes it a very important chapter.

---

## 4) Core idea #1: what model routing is

**Model routing** means your application can decide:

- which provider to use
- which model to use
- when to swap one for another
- without rewriting the whole product

### Plain-English version

Model routing is the layer that says:

> “For this task, use this model.”

### Why that matters

Different tasks may need different tradeoffs:

- speed
- quality
- cost
- reasoning ability
- context size

### Memory hook

If Chapter 2 taught you how to **choose a model**, Chapter 5 teaches you how to **design your system so model choice remains flexible**.

---

## 5) Why routing matters in practice

Without routing, your code can become tightly stuck to one model provider.

### Bad architecture

```text
business logic → directly tied to one provider SDK
```

### Better architecture

```text
business logic → routing layer → selected provider/model
```

### Figure: tight coupling vs abstraction

```text
Bad:
App logic ─────────► One provider forever

Better:
App logic ─► routing layer ─► provider/model A
                          └► provider/model B
                          └► provider/model C
```

### Why this matters

A routing layer makes it easier to:

- experiment with providers
- optimize costs later
- switch when a better model appears
- avoid rebuilding your whole app every time the model market changes

---

## 6) The practical reason routing exists

Model routing solves a very real engineering problem:

> model ecosystems change fast

That means:

- new providers appear
- old models become deprecated
- pricing changes
- latency changes
- new reasoning models arrive
- some models get better structured output support
- some models become better for code, search, or extraction

If your app is rigidly glued to one exact model, adapting becomes painful.

Routing keeps your system adaptable.

---

## 7) Code example from the chapter: routing with an SDK abstraction

The chapter shows an example where the model is injected into an agent through an SDK adapter:

```ts
import { openai } from '@ai-sdk/openai';
import { Agent } from '@mastra/core/agent';

const agent = new Agent({
  name: 'weather-agent',
  instructions: 'Instructions for the agent...',
  model: openai('gpt-4-turbo'),
});

// Use the agent
const result = await agent.generate('What is the weather like?');
```

### What this example is teaching

The important idea is **not** the weather task.
The important idea is that the model sits behind a common interface.

So instead of hardcoding your whole app around one provider, you can swap the model more easily.

---

## 8) What the routing example really means

The example is simple, but the deeper lesson is:

```text
Agent logic and model choice should be separable.
```

That way:

- your agent stays conceptually the same
- only the selected model changes

### Real-life analogy

Think of the model like a battery pack in a device.
You want to replace the battery without rebuilding the device.

---

## 9) Typical reasons to route models

The chapter hints at several practical reasons:

### 1. Strong model first, cheaper later

Use the strongest model while prototyping.
Swap to cheaper models later if quality still holds.

### 2. Task-specific model choice

Some tasks need:

- speed
- huge context
- structured output reliability
- reasoning

### 3. Provider flexibility

You may want to compare:

- OpenAI
- Anthropic
- Google
- open-source alternatives

### 4. Fallback behavior

If one model is unavailable or underperforming, a routing layer helps you recover more easily.

---

## 10) A simple router example (conceptual)

Here is a tiny router that makes the chapter’s idea stick:

```python
def choose_model(task_type: str) -> str:
    if task_type == "simple_qa":
        return "cheap_fast_model"
    elif task_type == "complex_reasoning":
        return "strong_reasoning_model"
    elif task_type == "document_analysis":
        return "large_context_model"
    return "default_model"
```

### Why this is useful

This is the whole chapter’s routing idea in tiny form:

- different tasks
- different best models

---

## 11) Core idea #2: what structured output is

The second half of the chapter is about **structured output**.

That means asking the model to return:

- JSON
- typed objects
- data shaped according to a schema

instead of loose free-form text.

### Example

Instead of getting:

```text
Machine learning is a field of AI. Some examples are image recognition and recommendation systems.
```

you ask for:

```json
{
  "definition": "Machine learning is a field of AI...",
  "examples": ["image recognition", "recommendation systems"]
}
```

That is structured output.

---

## 12) Why structured output matters

Structured output matters because applications need outputs that are:

- predictable
- parseable
- stable
- easy to validate
- safe for downstream code

### Unstructured text is great for:

- humans reading
- brainstorming
- open-ended explanations

### Structured output is great for:

- automation
- application logic
- workflows
- UI rendering
- storing results
- passing results into tools

This is the central engineering lesson.

---

## 13) Figure: prose vs structure

```text
Loose prose:
  "Here are some possible answers..."

Structured:
  {
    "category": "...",
    "priority": "...",
    "next_action": "..."
  }
```

### Memory hook

Prose is for people.  
Structured output is for systems.

---

## 14) Code example from the chapter: schema-driven output

The chapter uses **Zod** to define a schema:

```ts
import { z } from 'zod';

const mySchema = z.object({
  definition: z.string(),
  examples: z.array(z.string()),
});

const response = await llm.generate('Define machine learning and give examples.', {
  output: mySchema,
});

console.log(response.object);
```

### What this teaches

You do not merely say:

> “Please return JSON.”

You define the expected shape.

That is much more reliable.

---

## 15) Why “please output valid JSON” is not enough

A common beginner move is:

```text
Return valid JSON.
```

That helps a little, but it is weaker than real schema enforcement.

### Why?

Because “valid JSON” still leaves many problems:

- missing fields
- wrong field names
- wrong types
- extra junk
- partial structures

### Example of valid JSON that is still bad

```json
{
  "stuff": "something"
}
```

That is valid JSON, but useless if your app expected:

```json
{
  "definition": "...",
  "examples": ["...", "..."]
}
```

### Core lesson

The real goal is not:

```text
valid JSON
```

The real goal is:

```text
the right shape
```

---

## 16) Structured output turns LLMs into application components

This is the deep lesson of the chapter.

Once outputs are structured, you can:

- branch workflows
- trigger actions
- render UI
- save records
- run validations
- score results in evals

### Figure: where structured output helps

```text
LLM output
   ↓
validated schema object
   ├─ workflow decision
   ├─ UI display
   ├─ database save
   ├─ tool input
   └─ evaluation
```

This is one of the reasons structured output is such a major production concept.

---

## 17) Real-life examples of structured output

### Example 1: resume extraction

Input: resume text

Desired output:

```json
{
  "name": "Jane Doe",
  "skills": ["Python", "SQL"],
  "years_experience": 6
}
```

### Example 2: support ticket triage

Input: customer support request

Desired output:

```json
{
  "category": "billing",
  "priority": "high",
  "needs_human_escalation": true
}
```

### Example 3: meeting notes extraction

Input: meeting transcript

Desired output:

```json
{
  "decisions": ["..."],
  "action_items": ["..."],
  "owners": ["..."]
}
```

All of these are exactly the type of thing this chapter is preparing you to build.

---

## 18) Structured output and reliability

This chapter is quietly teaching a major rule of AI engineering:

> The more your output is consumed by code, the more it should be structured.

### Why

Because code is not good at “guessing what the model probably meant.”

### Better architecture

```text
human-facing answer → prose can be okay
machine-facing answer → use schema
```

---

## 19) Routing + structured output together

This chapter is powerful because it combines two ideas that work together:

### Routing decides:

- which model handles the task

### Structured output decides:

- what shape the result must have

### Figure: the combined logic

```text
Task arrives
   ↓
Route to best model
   ↓
Generate output under schema constraint
   ↓
Validated object returned to application
```

That is a very important production pattern.

---

## 20) Why this chapter matters for agents

Agents need both ideas constantly.

### They need routing because:

- different steps may need different models
- different costs/latencies matter
- systems should stay flexible

### They need structured output because:

- decisions must be machine-readable
- tool arguments must be correct
- workflows depend on predictable fields
- downstream systems expect stable data

So Chapter 5 is not just a chapter about “nice extras.”
It is really a chapter about the foundations of **reliable agent architecture**.

---

## 21) What routing does _not_ mean

The chapter is not saying:

- route constantly for no reason
- overcomplicate your first prototype
- optimize every model choice from day one

The better interpretation is:

- build with an abstraction layer
- preserve optionality
- optimize later without rewriting everything

---

## 22) What structured output does _not_ mean

Structured output does **not** mean:

- every response must be JSON
- humans should only see schemas
- prose is useless

It means:

- whenever software needs to consume the output, structure becomes very valuable

### Simple rule

| Output consumer           | Best default                |
| ------------------------- | --------------------------- |
| human                     | prose often okay            |
| machine / workflow / tool | structured output preferred |

---

## 23) Tiny code examples that make the chapter stick

### Example A: provider abstraction

```python
def call_model(provider, model_name, prompt):
    return {
        "provider": provider,
        "model": model_name,
        "result": f"Processed: {prompt}"
    }
```

### Lesson

Model/provider is now a parameter, not hardcoded architecture.

---

### Example B: simple routing

```python
def route_request(task):
    if task == "classification":
        return "small_fast_model"
    elif task == "analysis":
        return "strong_model"
    elif task == "long_document":
        return "large_context_model"
    return "default_model"
```

### Lesson

Different tasks justify different models.

---

### Example C: structured output validation idea

```python
expected_keys = {"definition", "examples"}

result = {
    "definition": "Machine learning is...",
    "examples": ["image recognition", "recommendation"]
}

assert expected_keys.issubset(result.keys())
```

### Lesson

Structure gives your application something stable to work with.

---

## 24) Common beginner mistakes this chapter prevents

### Mistake 1

“Let’s just hardcode one provider everywhere.”

### Better

Use a routing/abstraction layer so model choice stays flexible.

---

### Mistake 2

“JSON output means I just say ‘return JSON’.”

### Better

Use real schemas when possible.

---

### Mistake 3

“Structured output is only for advanced teams.”

### Better

Structured output is often one of the fastest ways to make an app more reliable.

---

### Mistake 4

“I’ll optimize model choice later, so I don’t need routing now.”

### Better

Even a light abstraction early can save a lot of pain later.

---

### Mistake 5

“Prose is always enough.”

### Better

Prose may be fine for humans, but systems usually want structure.

---

## 25) The chapter as a decision tree

```text
Building an LLM-powered app?
   │
   ├─ Will code consume the result?
   │      └─ Yes → use structured output
   │
   ├─ Will you want to compare/swap models later?
   │      └─ Yes → add routing/abstraction
   │
   ├─ Different tasks with different needs?
   │      └─ Yes → route by task type
   │
   ├─ Need machine-readable answers?
   │      └─ Yes → define schema
   │
   └─ Need reliable workflows/tools?
          └─ Routing + schemas are both important
```

---

## 26) Table: all major concepts in Chapter 5

| Concept                    | Meaning                                  | Why it matters                          |
| -------------------------- | ---------------------------------------- | --------------------------------------- |
| model routing              | choosing/swapping model via abstraction  | avoids lock-in, enables experimentation |
| provider abstraction       | app logic not tightly coupled to one SDK | flexibility                             |
| structured output          | schema-shaped machine-readable response  | reliability                             |
| schema                     | expected response shape                  | validation and downstream use           |
| Zod                        | schema library used in example           | typed structured output                 |
| JSON-like output           | machine-consumable response              | workflows/UI/storage                    |
| routing + schemas together | choose the brain and choose the shape    | production architecture pattern         |

---

## 27) What Chapter 5 is _not_ trying to do

This chapter is not yet going deep into:

- full workflow orchestration
- eval systems
- retry loops
- middleware enforcement
- advanced routing strategies
- failure recovery patterns

It is giving you the foundation:

- flexible model choice
- reliable output shape

That is enough to prepare for the next layers.

---

## 28) Best concise explanation of Chapter 5

If you had to explain the whole chapter in 30 seconds:

> Model routing means your app can choose and swap models/providers without being tightly tied to one SDK. Structured output means the model returns data in a defined schema instead of loose prose. Routing gives you flexibility, and structured output gives you reliability. Together they are key foundations for building real LLM applications and agents.

---

## 29) Final review sheet

### Must-remember facts

- Routing keeps model choice flexible.
- Structured output makes model responses application-friendly.
- Schemas matter more than “please return JSON.”
- Routing and structured output are both core production concepts.

### Must-remember mental model

```text
Routing = choose the brain
Structured output = choose the shape
```

### Must-remember builder lesson

The moment an LLM output is meant to drive code, UI, workflows, or tools, you should stop thinking only in terms of prompts and start thinking in terms of:

- abstraction layers
- schemas
- predictable interfaces

That is the deep practical lesson of Chapter 5.
