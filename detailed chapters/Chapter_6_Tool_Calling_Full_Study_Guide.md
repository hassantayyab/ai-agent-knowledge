# Chapter 6 — Tool Calling

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 6** clearly and memorably using diagrams, tables, examples, and code.

---

## 1) Summary of the chapter

Chapter 6 explains one of the most important ideas in modern AI agents:

> An agent becomes truly useful when it can **call tools** instead of only generating text.

This chapter teaches six major ideas:

1. A tool is a **function an agent can call** to do something real.
2. Tool calling is how an agent moves from “talking” to **acting**.
3. The hardest part is not coding the function, but **describing it well**.
4. Good tools need:
   - strong descriptions
   - clear names
   - strict input schemas
   - strict output schemas
5. Tool design is one of the most important parts of AI application design.
6. Real-world systems become much better when you break tasks into **reusable operations** instead of dumping everything into context.

That is the whole chapter in one line:

```text
Tool calling turns an LLM from a text generator into a system that can do real work.
```

---

## 2) The chapter in one figure

```text
User asks something
        │
        ▼
Agent decides:
"Can I answer from memory/text alone?"
        │
        ├─ Yes → answer directly
        │
        └─ No → call tool
                  │
                  ▼
           Tool does real action
                  │
                  ▼
           Tool returns structured result
                  │
                  ▼
           Agent uses result in answer
```

---

## 3) Core idea #1: what a tool is

A tool is a function that an agent can call.

### Examples of tools

- get weather information
- search a knowledge base
- query a database
- calculate something
- fetch a customer record
- create a ticket
- send an email
- retrieve invoice status

### Why this matters

Without tools, a model can only:

- generate text
- guess
- imitate knowledge from training

With tools, a model can:

- fetch live data
- interact with systems
- perform deterministic operations
- act in the real world

That is a huge shift.

---

## 4) Tool calling is what makes agents useful

This chapter is really about the difference between:

```text
"I sound helpful"
```

and

```text
"I can actually do something"
```

### Before tool calling

The model is mostly:

- a generator
- an explainer
- an imitator of tasks

### After tool calling

The model can:

- pull real facts
- work with real systems
- support workflows
- help automate business operations

### Real-life analogy

A smart employee with no system access is limited.  
A smart employee with the right software access becomes powerful.

Tool calling is that software access layer.

---

## 5) Core idea #2: the real problem is communication

This is one of the deepest lessons in the chapter.

The chapter says tool calling is mostly about **communication**:

- what the tool does
- when to use it
- what inputs it expects
- what outputs it returns

### Why this is such a big deal

A lot of beginners think the hard part is:

> “How do I wire up the function?”

But the chapter is saying the harder part is:

> “How do I explain this function clearly enough that the model knows when and how to use it?”

That is a major mental shift.

---

## 6) Figure: the model chooses tools using language

```text
Tool code exists
      │
      ▼
Model does not read your intentions
      │
      ▼
Model sees:
- tool name
- description
- input schema
- output schema
      │
      ▼
That description determines whether it gets used correctly
```

### Memory hook

For tools, **description is part of the code**.

---

## 7) Core idea #3: tool design is one of the most important steps

The chapter explicitly says that one of the most important things in AI app design is to first ask:

1. What is the full list of tools you need?
2. What will each tool do?

This is extremely important.

### Why?

Because a weak tool layer leads to:

- agent confusion
- wrong tool selection
- bad outputs
- wasted context
- hard-to-debug failures

### Better process

Before writing code, write the **tool map**.

---

## 8) Tool map example

Suppose you are building a support agent.

### Weak approach

One giant tool:

- `do_support_thing()`

### Better approach

Several specific tools:

- `lookup_customer_account`
- `get_invoice_status`
- `search_help_center`
- `create_support_ticket`
- `check_refund_eligibility`

### Why the second approach is better

Because each tool:

- has a clear purpose
- is easier for the model to choose
- is easier for you to debug
- is easier to evolve later

---

## 9) Core idea #4: best practices for tool design

The chapter gives explicit best practices.

### 1. Detailed descriptions

The tool should clearly say:

- what it does
- what it is for
- when it should be used

### 2. Specific input/output schemas

Schemas reduce ambiguity.

### 3. Semantic names

Use names that describe the action clearly.

### Bad name

```text
doStuff
```

### Better name

```text
getWeatherInformation
```

This seems obvious, but it matters a lot because the model is deciding through language.

---

## 10) Why semantic naming matters

Think like this:

A human engineer seeing:

```text
doStuff()
```

already hates it.

A model seeing it will hate it even more.

### Better naming gives:

- clearer selection
- lower ambiguity
- stronger match between request and function

### Table: bad vs good names

| Bad           | Better                    |
| ------------- | ------------------------- |
| `doThing`     | `getWeatherInformation`   |
| `tool1`       | `searchKnowledgeBase`     |
| `processData` | `extractInvoiceLineItems` |
| `runTask`     | `createSupportTicket`     |

### Memory hook

Good tool names are like good street signs.
They help the model take the right turn.

---

## 11) Code example from the chapter

The chapter gives a concrete Mastra example:

```ts
import { createTool } from '@mastra/core/tools';
import { z } from 'zod';

const getWeatherInfo = async (city: string) => {
  // Replace with an actual API call to a weather service
  console.log(`Fetching weather for ${city}...`);
  // Example data structure
  return { temperature: 20, conditions: 'Sunny' };
};

export const weatherTool = createTool({
  id: 'Get Weather Information',
  description: 'Fetches the current weather information for a given city',
  inputSchema: z.object({
    city: z.string().describe('City name'),
  }),
  outputSchema: z.object({
    temperature: z.number(),
    conditions: z.string(),
  }),
  execute: async ({ context: { city } }) => {
    console.log('Using tool to fetch weather information for', city);
    return await getWeatherInfo(city);
  },
});
```

---

## 12) What this code example teaches

This example contains almost every important idea in the chapter.

### Tool ID / name

```ts
id: 'Get Weather Information';
```

This gives the tool a human-readable identity.

### Description

```ts
description: 'Fetches the current weather information for a given city';
```

This tells the model what the tool is for.

### Input schema

```ts
inputSchema: z.object({
  city: z.string().describe('City name'),
});
```

This constrains the arguments.

### Output schema

```ts
outputSchema: z.object({
  temperature: z.number(),
  conditions: z.string(),
});
```

This constrains the returned data.

### Execute function

```ts
execute: async ({ context: { city } }) => ...
```

This is the actual operation.

### Important lesson

A tool is not just a function.
A tool is:

```text
function + description + schema + identity
```

---

## 13) Figure: anatomy of a good tool

```text
Tool
├─ Name / ID
├─ Description
├─ Input schema
├─ Output schema
└─ Execute function
```

If any one of these is weak, the tool becomes weaker.

---

## 14) Why input schemas matter

Input schemas matter because they tell the model:

- which fields exist
- what type they are
- how to format them

### Without schema

The model may invent:

- wrong keys
- wrong types
- vague arguments
- malformed payloads

### With schema

The tool call becomes more predictable.

### Example

Without schema:

```json
{ "place": "Berlin" }
```

With schema expecting `city`:

```json
{ "city": "Berlin" }
```

That difference matters in production.

---

## 15) Why output schemas matter

Output schemas are just as important.

### Why?

Because the model needs to know what the tool returns.

If the return shape is clear, the model can:

- reason about it better
- summarize it better
- chain it into other actions more reliably

### Example

Bad output expectation:

```text
returns some weather details
```

Better:

```json
{
  "temperature": 20,
  "conditions": "Sunny"
}
```

### Memory hook

Input schemas help the model **speak to the tool**.  
Output schemas help the model **understand the tool**.

---

## 16) Tool calling loop

A helpful mental model for this chapter is the tool-calling loop:

```text
User asks question
      ↓
Agent decides whether it needs a tool
      ↓
Tool chosen based on language + schemas
      ↓
Tool called with structured args
      ↓
Tool returns structured result
      ↓
Agent uses result in its answer or next step
```

This loop is the beating heart of many useful agents.

---

## 17) Why tools are different from just stuffing context

One of the strongest lessons in the chapter comes from the Alana example.

### The naive idea

Put all the information into the prompt and hope the model sorts it out.

### The better idea

Turn the work into operations:

- search by genre
- get investor recommendations
- filter by type
- sort results

### Why this is better

Because tools:

- reduce confusion
- reduce prompt overload
- make the task more modular
- let the model operate instead of merely “stare at a giant text blob”

That is a huge production insight.

---

## 18) The Alana book recommendation example

This chapter’s real-world example is excellent.

### First attempt

Alana put all the book recommendation information directly into the context window.

### Problem

The model did not reason about the data well enough.

### Better approach

Break the problem into reusable tools:

- access investor corpus
- get recommendations
- get books tagged by genre
- sort recommenders by type

### The deeper lesson

Do not force the model to solve everything from one giant prompt.
Instead:

- break the work into queries and operations
- expose those as tools
- let the model use them

---

## 19) Figure: giant prompt vs analyst-style tool design

```text
Bad:
Huge pile of data in prompt
        ↓
"Please figure everything out"

Better:
Question
   ↓
Use tool A
   ↓
Use tool B
   ↓
Combine results
   ↓
Answer
```

### Memory hook

The agent should think like an analyst, not like a person drowning in a giant PDF.

---

## 20) Tool design as analyst operations

This is maybe the single most important practical insight in the chapter.

### Ask:

What would a smart analyst do manually?

Examples:

- search by category
- filter by condition
- retrieve records
- compute aggregate
- sort by priority
- create/update ticket
- compare documents

Then:

> turn those analyst moves into tools

This is an incredibly useful design method.

---

## 21) Real-life examples that make the chapter stick

### Example 1: weather assistant

Bad approach:

- LLM guesses weather from training

Better approach:

- tool fetches live weather API
- model explains result

### Example 2: enterprise support agent

Bad approach:

- dump all support docs into the prompt

Better approach:

- tool to search help center
- tool to look up account
- tool to check invoice status
- tool to open ticket

### Example 3: book recommendation system

Bad approach:

- giant blob of recommendations in context

Better approach:

- search/filter/sort/retrieve as separate tools

---

## 22) Tiny code examples that make the chapter memorable

### Example A: a very bad tool

```python
def do_stuff(x):
    return something(x)
```

Why it is bad:

- bad name
- unclear purpose
- no schema
- no defined output structure

---

### Example B: a clearer tool

```python
def get_invoice_status(invoice_id: str) -> dict:
    return {
        "invoice_id": invoice_id,
        "status": "paid",
        "due_date": "2025-05-01"
    }
```

Why it is better:

- clear name
- clear input
- clearer output

---

### Example C: schema-like thinking

```python
tool_input = {
    "city": "Berlin"
}

tool_output = {
    "temperature": 20,
    "conditions": "Sunny"
}
```

This is simple, but it captures the chapter’s structured mindset.

---

## 23) Common beginner mistakes this chapter prevents

### Mistake 1

“The hard part is coding the function.”

### Better

The hard part is often clearly describing and structuring the tool.

---

### Mistake 2

“One giant tool is easier.”

### Better

Smaller, clearer tools are usually easier for the model and easier to debug.

---

### Mistake 3

“I can just give the model all the data in the prompt.”

### Better

Often better to expose data access as reusable tools.

---

### Mistake 4

“Names don’t matter much.”

### Better

Names matter a lot because the model makes decisions through language.

---

### Mistake 5

“Only input schema matters.”

### Better

Output schema matters too, because the agent needs to understand what comes back.

---

## 24) The chapter as a decision tree

```text
Need the agent to do real work?
   │
   ├─ Does it require fresh data or system access?
   │      └─ Yes → define tool
   │
   ├─ Is the task currently one giant prompt?
   │      └─ Yes → break into operations
   │
   ├─ Is tool choice ambiguous?
   │      └─ Improve names + descriptions
   │
   ├─ Are tool arguments inconsistent?
   │      └─ Add stronger input schema
   │
   └─ Are returned results hard to use?
          └─ Add stronger output schema
```

---

## 25) Table: all major concepts in Chapter 6

| Concept              | Meaning                               | Why it matters                              |
| -------------------- | ------------------------------------- | ------------------------------------------- |
| tool                 | callable function for the agent       | turns text generation into action           |
| tool calling         | model chooses and uses tools          | foundation of useful agents                 |
| description          | language explanation of tool purpose  | helps model choose correctly                |
| semantic naming      | tool names that match the job         | reduces ambiguity                           |
| input schema         | expected arguments                    | improves correctness                        |
| output schema        | expected return shape                 | improves downstream reasoning               |
| analyst operations   | reusable task steps turned into tools | best design method in the chapter           |
| giant prompt failure | stuffing everything into context      | less reliable than tool-based decomposition |

---

## 26) What Chapter 6 is _not_ trying to do

This chapter is not yet deeply teaching:

- retries and fallback logic
- tool security middleware
- evaluation of tool calling
- multi-step workflow orchestration
- memory interactions
- production observability

It is laying the conceptual foundation:

> what tools are, why they matter, and how to design them well

Later chapters build on this.

---

## 27) Best concise explanation of Chapter 6

If you had to explain the whole chapter in 30 seconds:

> Tool calling is what lets an LLM agent actually do things instead of only generating text. A tool is more than just a function: it also needs a clear name, description, and strict schemas for inputs and outputs. Good tool design is one of the most important parts of building AI applications. Instead of dumping everything into a giant prompt, break work into reusable analyst-style operations and expose those as tools.

---

## 28) Final review sheet

### Must-remember facts

- Tool calling turns agents into action systems.
- Tool descriptions and names matter a lot.
- Input and output schemas are both important.
- Tool design is one of the most important stages of AI app design.
- Reusable operations beat giant context blobs.

### Must-remember mental model

```text
Tool = action interface the model can understand and use
```

### Must-remember builder lesson

Before building an agent, ask:

> “What are the reusable operations a human analyst would perform here?”

Then turn those operations into tools.

That is the deep practical lesson of Chapter 6.
