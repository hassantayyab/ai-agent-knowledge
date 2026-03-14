# Chapter 7 — Agent Memory

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 7** clearly and memorably using diagrams, tables, examples, and code.

---

## 1) Summary of the chapter

Chapter 7 explains why **memory** is essential for useful agents.

The chapter teaches six core ideas:

1. Agents need memory because single LLM calls are not enough for long, contextual conversations.
2. **Working memory** stores persistent user characteristics and relevant long-term details.
3. **Hierarchical memory** combines recent conversation context with relevant long-term recalled context.
4. Good memory systems are **selective**, not “dump everything into the prompt.”
5. Memory systems need controls for context size, which the chapter calls **memory processors**.
6. Two concrete processor examples are:
   - `TokenLimiter`
   - `ToolCallFilter`

That is the whole chapter in one line:

```text
Good agent memory = recent context + relevant recalled history, filtered so the model gets only what matters.
```

---

## 2) The chapter in one figure

```text
User sends message
        │
        ▼
Recent messages kept
        +
Relevant older memories recalled
        │
        ▼
Combined into context window
        │
        ▼
Filtered / pruned if necessary
        │
        ▼
Agent responds with the right context
```

---

## 3) Why memory matters

The chapter opens by saying memory is crucial for creating agents that maintain meaningful, contextual conversations over time.

### Why?

Because LLMs are good at handling:

- one message
- one prompt
- one reply

But they need help with:

- longer-term context
- historical interactions
- persistent user details
- remembering what matters from earlier

### Plain-English version

A model without memory is like a smart person with amnesia.
It may answer well in the moment, but it cannot build continuity over time.

---

## 4) Memory is not the same as context window stuffing

A beginner might think:

```text
If I want memory, I should just keep appending everything forever.
```

The chapter strongly pushes against this.

### Why not?

Because appending everything:

- wastes tokens
- causes context overflow
- includes irrelevant old information
- makes the system slower and less reliable

### Better idea

A good memory system:

- keeps the recent conversation
- selectively recalls older relevant context
- leaves irrelevant history out

That is the big design lesson of the chapter.

---

## 5) Core idea #1: working memory

The chapter first introduces **working memory**.

### Definition

Working memory stores relevant, persistent, long-term characteristics of users.

### Example from the chapter

The chapter gives a very practical example:

- ask ChatGPT what it knows about you

That shows the idea of persistent remembered characteristics.

### What kind of things fit in working memory?

- user preferences
- communication style
- recurring constraints
- stable details about the user
- relevant long-term facts

### Memory hook

Working memory is:

```text
what the system “knows about you” across time
```

---

## 6) Real-life example of working memory

Suppose an assistant remembers:

- you prefer concise answers
- you work in engineering
- you like bullet points
- your main project is internal support automation

Then later, it can answer better without asking the same setup questions repeatedly.

That is the usefulness of working memory.

---

## 7) Figure: working memory

```text
User
  ├─ prefers concise answers
  ├─ works on support automation
  ├─ likes markdown
  └─ often asks about internal docs

These stable characteristics
        ↓
stay available across conversations
```

### Important point

Working memory is not everything about the user.
It is the **relevant persistent stuff**.

---

## 8) Core idea #2: hierarchical memory

The chapter next introduces **hierarchical memory**.

This sounds fancy, but the chapter explains it in a very simple way:

- use recent messages
- combine them with relevant long-term memories

### That is it.

Hierarchical memory means:

```text
recent context + relevant past context
```

That is the whole idea.

---

## 9) The chapter’s explanation of hierarchical memory

The chapter gives a simple conversational example:

If someone asks:

> “What did you do last weekend?”

A memory system would:

1. search memory for relevant events from last weekend
2. look at the recent conversation
3. combine both into the context window
4. then answer

This is a very good beginner example because it makes the process concrete.

### Figure: hierarchical recall

```text
Question arrives
     │
     ▼
Search long-term memory for relevant old info
     │
     ▼
Check recent conversation
     │
     ▼
Join both into current context window
     │
     ▼
Generate answer
```

---

## 10) Why hierarchical memory is better than naive memory

A naive system might include:

- the whole conversation
- all prior sessions
- every previous tool output
- every old detail

That is usually bad.

A hierarchical system instead asks:

- what just happened recently?
- what older memory is relevant right now?

This is a much smarter and cheaper approach.

### Table: naive vs hierarchical memory

| Naive memory           | Hierarchical memory        |
| ---------------------- | -------------------------- |
| shove everything in    | retrieve only what matters |
| expensive              | more efficient             |
| noisy                  | more relevant              |
| risks context overflow | helps control context size |

---

## 11) Memory retrieval process

The chapter gives a concrete mental model:

```text
Conversation history exists
        │
        ▼
Take recent sliding window
        │
        ▼
Search older messages semantically
        │
        ▼
Retrieve only the most relevant matches
        │
        ▼
Add them into context for the current query
```

This is one of the most important practical patterns in agent memory systems.

---

## 12) The Mastra memory options example

The chapter uses a code example to show how this works.

```ts
await agent.stream('What did we decide about the search feature last week?', {
  memoryOptions: {
    lastMessages: 10,
    semanticRecall: {
      topK: 3,
      messageRange: 2,
    },
  },
});
```

This short example contains most of the chapter’s important practical memory design ideas.

---

## 13) What each memory option means

### `lastMessages`

Keeps a sliding window of the most recent messages.

### Why it matters

This ensures the agent sees the immediate conversational context.

### Memory hook

```text
lastMessages = short-term conversational memory
```

---

### `semanticRecall`

This means the system uses semantic search, basically a memory-RAG approach, over older conversations.

### Why it matters

Instead of manually scanning everything, the system retrieves older messages that are semantically relevant to the current question.

### Memory hook

```text
semanticRecall = search old memory by meaning
```

---

### `topK`

This is the number of relevant memory results to retrieve.

### Why it matters

It keeps retrieval limited and manageable.

### Simple idea

If `topK = 3`, the system only brings back the three most relevant memory hits.

---

### `messageRange`

This includes surrounding messages near a relevant match.

### Why this matters

Sometimes the single matching message is not enough.
You also need the nearby messages for context.

### Example

If the relevant memory hit was:

> “We chose Redis for caching.”

The messages before and after might explain:

- why that choice was made
- what constraints mattered
- what alternatives were rejected

That makes the recalled memory more useful.

---

## 14) Figure: how memory retrieval works

```text
Full conversation history
[old][old][old][match][nearby][nearby][old][old]

Current question
        ↓
Semantic search finds "match"
        ↓
messageRange pulls nearby messages too
        ↓
Selected memory inserted into current context
```

This is a very important pattern to remember.

---

## 15) Why selective retrieval matters

The chapter explicitly says:

- do not overwhelm the model with the entire conversation history
- selectively include only the most relevant past interactions
- this prevents context window overflow while preserving relevance

That is one of the most practical lessons in the chapter.

### Simple rule

```text
Memory quality is about relevance, not volume.
```

---

## 16) Growing context windows do not remove memory design

The chapter also includes an interesting note:

As context windows grow, developers often start by throwing everything into the context window and worry about memory later.

### Why this note matters

It is a warning.

A large context window can delay the need for memory engineering, but it does not eliminate it.

### Why?

Because even huge context windows still create issues:

- cost
- noise
- retrieval precision
- unnecessary token use
- degraded reasoning quality from irrelevant context

### Builder lesson

A bigger context window is not a substitute for a good memory strategy.

---

## 17) Core idea #3: memory processors

The chapter then introduces **memory processors**.

### What they are

Memory processors modify the set of retrieved memory messages **before** they are added to the context window.

### Plain-English version

They are filters or controllers for memory.

### Why they matter

Sometimes the right move is not:

```text
make the context bigger
```

Sometimes the right move is:

```text
make the context cleaner
```

That is the whole reason memory processors exist.

---

## 18) Why memory processors are important

Memory processors help with:

- managing context size
- filtering content
- optimizing performance
- deciding what the model really needs to see

### Figure: memory processors in the pipeline

```text
Retrieved memory
      │
      ▼
Memory processors
  ├─ prune old content
  ├─ filter tool calls
  ├─ custom logic
      │
      ▼
Cleaned memory context
      │
      ▼
Sent to LLM
```

### Memory hook

Processors are like editors for memory.

---

## 19) Core idea #4: TokenLimiter

The chapter’s first built-in processor is `TokenLimiter`.

### What it does

It counts the tokens in retrieved memory messages and removes the oldest ones until the total count is below a chosen limit.

### Why this matters

This prevents context-window-limit errors.

### Plain-English version

If memory is too big, `TokenLimiter` trims it.

### Figure

```text
Retrieved memory too large?
      │
      ▼
Count tokens
      │
      ▼
Remove oldest messages
      │
      ▼
Fit under token limit
```

This is a very practical production feature.

---

## 20) Code example: TokenLimiter

```ts
import { Memory } from '@mastra/memory';
import { TokenLimiter } from '@mastra/memory/processors';
import { Agent } from '@mastra/core/agent';
import { openai } from '@ai-sdk/openai';

const agent = new Agent({
  model: openai('gpt-4o'),
  memory: new Memory({
    processors: [
      // Ensure the total tokens from memory don't exceed ~127k
      new TokenLimiter(127000),
    ],
  }),
});
```

### What this teaches

Memory systems need actual engineering constraints.
They are not just conceptual.

---

## 21) Real-life analogy for TokenLimiter

Imagine you are going into a meeting with too many papers.

You do not bring:

- every email
- every draft
- every old note

You trim down the stack to what fits in the folder.

That is exactly what `TokenLimiter` does for memory.

---

## 22) Core idea #5: ToolCallFilter

The second processor the chapter explains is `ToolCallFilter`.

### What it does

It removes tool call messages from memory before those messages are sent to the LLM.

### Why this helps

Tool calls can be:

- verbose
- noisy
- token-heavy
- unnecessary in future context

### Practical benefit

This saves tokens and keeps the context cleaner.

---

## 23) Why ToolCallFilter is especially useful

The chapter gives an especially important reason:

If you want the agent to call a tool again, you may not want it to rely on stale previous tool results in memory.

### Example

Suppose a tool fetched:

- weather
- stock price
- ticket status
- invoice state

Those things can change.

If the old tool result is still in memory, the model might rely on it instead of calling the tool again.

That can be bad.

### Memory hook

```text
ToolCallFilter helps the agent re-check reality instead of trusting old tool traces.
```

---

## 24) Figure: why ToolCallFilter can help

```text
Past memory contains:
- long tool logs
- old API results
- verbose traces

ToolCallFilter removes them
        ↓
Model gets cleaner memory
        ↓
Can call tools again if needed
```

This is a subtle but important production design point.

---

## 25) Why memory is a retrieval problem, not just a storage problem

This is one of the hidden lessons of the chapter.

A lot of beginners think memory means:

> “store everything.”

But the chapter really teaches:

> Memory is mostly about deciding what to retrieve and include.

That means a good memory system is not only about saving data.
It is also about:

- selecting relevance
- controlling noise
- protecting context size
- deciding what the model should see right now

That is a much more advanced and useful understanding.

---

## 26) Real-life examples that make Chapter 7 stick

### Example 1: personal assistant

The user asks:

> “What travel preferences do I usually have?”

A good memory system recalls:

- past preferences
- maybe recent discussion about flights
  not the entire chat history

---

### Example 2: product planning assistant

The user asks:

> “What did we decide last week about the search feature?”

A good memory system:

- recalls the specific relevant decision
- includes nearby context
- uses recent messages too

That is almost exactly the chapter’s example pattern.

---

### Example 3: support agent

The user asks:

> “Did we already discuss refund eligibility?”

A good system:

- checks recent messages
- recalls older refund-related conversation
- excludes irrelevant unrelated history

---

## 27) Tiny code examples that make the chapter memorable

### Example A: simple recent window

```python
recent_messages = messages[-10:]
```

This captures the `lastMessages` idea.

---

### Example B: semantic recall idea

```python
def semantic_recall(query, memory_store, top_k=3):
    # pretend this searches old memories by meaning
    return memory_store.search(query, top_k=top_k)
```

This captures the `semanticRecall` idea.

---

### Example C: token trimming idea

```python
def trim_to_token_limit(memory_items, max_items=5):
    return memory_items[-max_items:]
```

This is a toy version of the idea behind `TokenLimiter`.

---

### Example D: filtering tool logs

```python
def remove_tool_calls(messages):
    return [m for m in messages if m.get("role") != "tool"]
```

This is a toy version of `ToolCallFilter`.

---

## 28) Common beginner mistakes this chapter prevents

### Mistake 1

“Memory means keep the whole conversation forever in the prompt.”

### Better

Memory should be selective.

---

### Mistake 2

“Large context windows make memory design irrelevant.”

### Better

Large context helps, but relevance and pruning still matter.

---

### Mistake 3

“If I store memory, the job is done.”

### Better

Storage is only half the problem.
Retrieval is the real challenge.

---

### Mistake 4

“Old tool results should always stay in context.”

### Better

Sometimes tool traces create noise or stale assumptions.

---

### Mistake 5

“Recent context and long-term memory are the same thing.”

### Better

They are different layers and should be handled differently.

---

## 29) The chapter as a decision tree

```text
Need an agent to remember things?
   │
   ├─ Does recent conversation matter?
   │      └─ Yes → keep lastMessages window
   │
   ├─ Does older relevant context matter?
   │      └─ Yes → use semanticRecall
   │
   ├─ Is too much memory being added?
   │      └─ Yes → use TokenLimiter
   │
   ├─ Are old tool traces cluttering context?
   │      └─ Yes → use ToolCallFilter
   │
   └─ Final goal:
          recent + relevant, not everything
```

---

## 30) Table: all major concepts in Chapter 7

| Concept             | Meaning                                             | Why it matters                          |
| ------------------- | --------------------------------------------------- | --------------------------------------- |
| memory              | long-term contextual continuity for agents          | enables ongoing useful conversation     |
| working memory      | persistent user characteristics / long-term context | personal continuity                     |
| hierarchical memory | combine recent messages + relevant recalled history | smarter than naive full history         |
| lastMessages        | sliding recent window                               | preserves immediate context             |
| semanticRecall      | search older conversations by meaning               | retrieves relevant memory               |
| topK                | number of retrieved matches                         | controls recall size                    |
| messageRange        | include nearby messages around a hit                | improves context coherence              |
| memory processors   | filters/modifiers before memory enters context      | controls size and quality               |
| TokenLimiter        | trims retrieved memory to fit token budget          | avoids overflow                         |
| ToolCallFilter      | removes tool call traces from memory                | reduces noise and stale tool dependence |

---

## 31) What Chapter 7 is _not_ trying to do

This chapter is not yet deeply teaching:

- vector database implementation
- embedding model choices
- long-term memory product policy
- full memory eval systems
- advanced summarization pipelines
- memory-sharing between multiple agents

It is giving the foundation:

- what agent memory is
- why selective retrieval matters
- how to control memory before it reaches the model

That is the right foundation for later work.

---

## 32) Best concise explanation of Chapter 7

If you had to explain the whole chapter in 30 seconds:

> Agent memory helps maintain contextual conversations over time. A good memory system combines a recent-message window with semantically recalled relevant older context, instead of stuffing everything into the prompt. Because too much memory can hurt performance, memory processors like TokenLimiter and ToolCallFilter help prune or filter context before it reaches the model. The key idea is that good memory is selective and relevance-driven.

---

## 33) Final review sheet

### Must-remember facts

- Memory is essential for contextual long-running agents.
- Working memory stores persistent user characteristics.
- Hierarchical memory = recent context + relevant recalled context.
- Selective retrieval beats dumping full history.
- Memory processors help manage context size and cleanliness.
- `TokenLimiter` trims memory.
- `ToolCallFilter` removes tool traces.

### Must-remember mental model

```text
Good memory = recent + relevant + filtered
```

### Must-remember builder lesson

Do not ask:

> “How do I store everything?”

Ask:

> “What should the model see right now to do the best job?”

That is the deep practical lesson of Chapter 7.
