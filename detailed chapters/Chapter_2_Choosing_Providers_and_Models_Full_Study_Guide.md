# Chapter 2 — Choosing Providers and Models

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 2** in a compact but memorable way using diagrams, tables, examples, and tiny code demos.

---

## 1) Summary of the chapter

Chapter 2 is about **how to choose the model behind your AI product**.

It covers five big ideas:

1. Why most builders should start with **hosted API providers** instead of self-hosting open-source models.
2. Why model choice is really a tradeoff between **accuracy, cost, and latency**.
3. Why **context window size** matters a lot more than beginners think.
4. What **reasoning models** are and how they differ from standard chat-style models.
5. A snapshot of the major providers and model families available at the time of the book.

That is the whole chapter in one line:

```text
Start with hosted APIs → choose quality first → optimize cost later → use context windows wisely → understand when reasoning models are worth it
```

---

## 2) The chapter in one figure

```text
                 Need a model for your app
                           │
                           ▼
         ┌────────────────────────────────────┐
         │ Start with hosted API providers    │
         └────────────────────────────────────┘
                           │
                           ▼
       Choose based on the main tradeoffs:
       ┌───────────────┬───────────────┬───────────────┐
       │ accuracy      │ cost          │ latency       │
       └───────────────┴───────────────┴───────────────┘
                           │
                           ▼
            Then also think about:
            ┌───────────────┬──────────────────┐
            │ context size  │ reasoning ability │
            └───────────────┴──────────────────┘
                           │
                           ▼
                 Pick best model now,
              cheaper/faster model later
```

---

## 3) Core idea #1: Start with hosted APIs

The chapter strongly recommends that most people building AI products should begin with **hosted model providers** such as:

- OpenAI
- Anthropic
- Google Gemini

instead of trying to immediately run open-source models themselves.

### Why the chapter recommends this

Because when you are early in development, your real problem is usually not infrastructure.
Your real problem is:

- figuring out what to build
- figuring out if the product actually works
- testing prompt and workflow designs
- learning what model quality you actually need

### Beginner mistake

A common mistake is:

```text
"I want full control, so I should self-host from day one."
```

The chapter argues that this usually slows you down.

### Better default

```text
Use hosted APIs first → prove the product works → optimize later
```

### Why this matters

Hosted APIs let you:

- iterate faster
- skip infra complexity
- compare models quickly
- avoid wasting time on GPU/serving problems too early

### Real-life analogy

If you are opening a restaurant, the first question is not:

> “Should I build my own farming supply chain?”

The first question is:

> “Do people want the food?”

That is how the chapter treats hosted APIs.

---

## 4) Hosted vs open-source: the real comparison

### Table: hosted vs open-source models

| Option                  | Advantages                                                        | Disadvantages                                      | Best use case                                   |
| ----------------------- | ----------------------------------------------------------------- | -------------------------------------------------- | ----------------------------------------------- |
| Hosted APIs             | fast setup, easy iteration, low ops burden                        | ongoing API cost, less infra control               | early product development                       |
| Open-source/self-hosted | more control, potential cost savings at scale, more customization | infra complexity, hosting burden, slower iteration | later-stage optimization or special constraints |

### What the chapter is _really_ teaching

It is not saying open-source is bad.  
It is saying:

> **Don’t optimize for infrastructure before you optimize for product correctness.**

That is a very important builder lesson.

---

## 5) Core idea #2: Model size creates tradeoffs

The chapter says that choosing a model usually means managing a tradeoff between:

- **accuracy**
- **cost**
- **latency**

### The basic rule

Bigger/stronger models tend to:

- perform better
- cost more
- run slower

Smaller models tend to:

- be cheaper
- be faster
- make more mistakes

### Figure: the core tradeoff triangle

```text
                Accuracy
                   ▲
                   │
                   │
                   │
     Latency ◄─────┼─────► Cost
```

You usually do **not** get the best point on all three at once.

---

## 6) What “cost” means here

When the chapter talks about cost, it mainly means:

- API cost per request
- total product cost at scale

### Why this matters

In a prototype, a stronger model might be perfectly fine.
In production, if you serve thousands or millions of requests, cost can become a major product constraint.

### Good mental model

At prototype stage:

```text
Correctness > cost
```

At scale:

```text
Correctness + cost + latency all matter
```

---

## 7) What “latency” means here

Latency means:

> how long the model takes to respond

### Why latency matters

A model that gives amazing answers but takes too long can still be bad for:

- chat UX
- customer support
- enterprise workflows
- real-time copilots

### Simple example

| Model type      | Quality | Speed  |
| --------------- | ------- | ------ |
| strongest model | high    | slower |
| smaller model   | lower   | faster |

So the real question becomes:

> “How much quality do I need for this task?”

---

## 8) The chapter’s practical advice: start with the best model

This is one of the most useful ideas in the chapter.

### The advice

Start with the **best model you can reasonably use** while prototyping.

Why?

Because at the beginning, you want to answer:

- does the product idea work?
- what does success actually look like?
- what quality level is required?

If you start with a weaker model too early, you may misdiagnose the problem.

### The danger of starting too cheap

You might think:

- the workflow is bad
- the prompt is bad
- the product is bad

when really:

- the model was just too weak

### Better process

```text
Step 1: prove the experience with a strong model
Step 2: understand the baseline quality
Step 3: try smaller/faster/cheaper replacements later
```

### Memory hook

Use the expensive microscope first.  
Once you know what you are looking at, you can decide if cheaper tools are enough.

---

## 9) Core idea #3: Context window size matters a lot

This is one of the most important practical model-selection ideas in the chapter.

### What is a context window?

A context window is the amount of text the model can “see” in one request.

That can include:

- user prompt
- system prompt
- chat history
- retrieved documents
- code files
- tool results

### Why bigger context windows help

A larger context window can let you:

- stuff in more documents
- include more code
- avoid building retrieval logic too early
- prototype faster

### Beginner-friendly explanation

A context window is like the size of the desk a model can work on.

- small desk = fewer papers visible
- huge desk = more papers visible at once

---

## 10) Why context size changes what is possible

The chapter gives an example of a very large context window, saying Gemini 1.5 Pro can support about **2 million tokens**, roughly **4,000 pages**.

### Why that matters

With that kind of context, you can do things like:

- put a whole support knowledge base into context
- include large codebases
- try simpler brute-force prototypes before building RAG

### Figure: small vs large context window

```text
Small context window
┌───────────────┐
│ prompt        │
│ a few docs    │
│ short history │
└───────────────┘

Large context window
┌──────────────────────────────────────┐
│ prompt                               │
│ long history                         │
│ many docs                            │
│ large codebase chunks                │
│ lots of examples                     │
└──────────────────────────────────────┘
```

### Builder lesson

A bigger context window can reduce engineering complexity in the first version of a product.

That means sometimes the best “engineering choice” is actually a model choice.

---

## 11) But large context windows are not magic

The chapter presents large context windows as useful, but the hidden lesson is:

> just because you can stuff in more context does not always mean you should

### Why not?

Because more context can also mean:

- more cost
- slower responses
- more irrelevant information
- noisier prompts

### Good practical interpretation

Large context windows are especially helpful for:

- rapid prototyping
- simpler first versions
- code understanding
- document-heavy workflows

Later, you may still want:

- RAG
- filtering
- chunking
- smarter retrieval

---

## 12) Core idea #4: reasoning models are different

The chapter introduces **reasoning models** as a distinct kind of model.

### How they are described

Reasoning models:

- take longer
- may spend more compute before answering
- may return answers “all at once”
- may show intermediate reasoning-like steps in some contexts

### Beginner-friendly summary

A reasoning model is like a model that is allowed to “work longer before speaking.”

### Figure: standard vs reasoning model behavior

```text
Standard model:
Prompt → answer quickly

Reasoning model:
Prompt → think longer / more compute → answer later
```

---

## 13) Why reasoning models need more context

The chapter says something very important:

> reasoning models need **lots of context and good examples**

This means they work better when you provide:

- strong background information
- good instructions
- many-shot examples
- the relevant material they need to reason over

### Why this matters

Beginners often assume:

> “Reasoning model = automatically better.”

But the chapter argues that they can still fail badly if you do not feed them the right setup.

### Memory hook

A reasoning model is more like a report writer than a casual chat assistant.

You cannot just say:

> “Do magic.”

You need to hand it a proper folder.

---

## 14) Reasoning models as report generators

This is one of the chapter’s most useful mental models.

### Think of them like:

- analysts
- report writers
- slower but more thorough assistants

### Best use cases

- complex synthesis
- hard reasoning tasks
- analysis over lots of context
- structured report generation

### Not ideal for

- super-fast UX
- tiny simple requests
- high-volume cheap interactions

---

## 15) Figure: choosing standard vs reasoning models

```text
Simple / fast / high-volume task?
        └─► standard model

Complex / deep / high-context task?
        └─► reasoning model
```

### Real-life example

#### Standard model task

“Write a friendly reply to this customer.”

#### Reasoning model task

“Analyze 20 pages of internal policy plus this incident report and explain the likely compliance risks.”

The second one benefits much more from a reasoning-style model.

---

## 16) The danger of under-contexting reasoning models

The chapter warns that if you do not give reasoning models enough context or examples, they can “go off the rails.”

### What that means

They may:

- reason in the wrong direction
- overcomplicate the task
- miss the intended frame
- produce impressive-looking but unhelpful output

### Key lesson

The better the model is at reasoning, the more harmful bad setup can become.

Why?

Because a weak model may fail simply.
A strong reasoning model may fail **confidently and elaborately**.

---

## 17) Model choice is not permanent

Another hidden lesson of the chapter:

> choosing a model is not a one-time forever decision

The chapter encourages you to use routing layers or abstraction layers so that you can:

- compare providers
- switch models
- optimize later
- avoid getting trapped too early

### Practical builder idea

Your application logic should ideally not be tightly welded to one exact model forever.

### Real-life analogy

Don’t pour the concrete around a single car engine before you know if that is the engine you want to keep.

---

## 18) The provider snapshot in the chapter

The chapter includes a table of providers and models from around **May 2025**.

The exact model names are less important than the pattern:

- each provider has stronger/default models
- each may have smaller or cheaper models
- some have reasoning-focused models
- builders often compare these tradeoffs

### High-level provider view

| Provider      | What the chapter emphasizes                         |
| ------------- | --------------------------------------------------- |
| OpenAI        | broad model range including reasoning-style options |
| Anthropic     | strong hosted provider                              |
| Google Gemini | major hosted provider with giant context windows    |
| Mistral       | open-source ecosystem player                        |
| Meta          | Llama open-source family                            |
| DeepSeek      | open-source player with reasoning presence          |
| Qwen          | open-source model family                            |

### The point of this table

It is not:

> “memorize every model SKU.”

It is:

> “understand the market categories and compare capabilities.”

---

## 19) Chapter 2’s real message in plain English

If you strip away all details, Chapter 2 is saying:

1. Don’t overcomplicate infrastructure too early.
2. Start with strong hosted models.
3. Learn what quality your product actually needs.
4. Use cheaper/faster models later if you can.
5. Context windows and reasoning ability matter just as much as model brand.

That is the true practical lesson.

---

## 20) Real-life examples that make Chapter 2 stick

### Example 1: startup support chatbot

A startup wants an AI support assistant.

#### Bad approach

- self-host open-source from day one
- weak model
- tiny context window
- poor support quality

They conclude:

> “AI support is not ready.”

But maybe the real problem was bad model choice.

#### Better approach

- use a strong hosted model
- include lots of support docs
- test the real experience first
- optimize cost later

### Example 2: legal/compliance assistant

A company wants a policy-analysis assistant.

This likely benefits from:

- large context window
- reasoning model
- many examples
- lots of documents in prompt

A cheap tiny model might completely miss the nuance.

### Example 3: high-volume email classifier

A company wants to classify millions of inbound emails.

This may favor:

- smaller cheaper model
- lower latency
- high throughput

Here, cost efficiency matters much more.

---

## 21) Table: all major concepts in Chapter 2

| Concept                        | Meaning                                   | Why it matters                            |
| ------------------------------ | ----------------------------------------- | ----------------------------------------- |
| hosted APIs                    | use provider-hosted models                | fastest way to prototype                  |
| open-source/self-hosting       | run models yourself                       | useful later, costly early                |
| accuracy/cost/latency tradeoff | better models cost more and run slower    | core model-selection problem              |
| strong-first prototyping       | start with best model                     | avoids false negatives                    |
| context window                 | how much text fits in one request         | affects what products are even possible   |
| large context                  | more docs/code/history in one go          | easier first versions                     |
| reasoning models               | slower, deeper, more compute-heavy models | useful for hard tasks                     |
| many-shot examples             | lots of examples in the prompt            | especially important for reasoning models |
| routing/abstraction            | keep model choice flexible                | enables later optimization                |

---

## 22) Tiny code examples to make the chapter memorable

### Example A: simple provider abstraction

```python
def call_model(provider, model_name, prompt):
    if provider == "openai":
        return f"[OpenAI:{model_name}] -> {prompt}"
    elif provider == "anthropic":
        return f"[Anthropic:{model_name}] -> {prompt}"
    elif provider == "google":
        return f"[Google:{model_name}] -> {prompt}"
    else:
        raise ValueError("Unknown provider")
```

### Why this matters

The chapter wants you to think in terms of:

- swappable providers
- swappable models
- experimentation

---

### Example B: simple model router

```python
def choose_model(task_type):
    if task_type == "simple_qa":
        return "cheap_fast_model"
    elif task_type == "document_analysis":
        return "large_context_model"
    elif task_type == "complex_reasoning":
        return "reasoning_model"
    return "default_model"
```

### Why this matters

This is the simplest way to remember the chapter:
different tasks deserve different models.

---

## 23) Common beginner mistakes Chapter 2 helps prevent

### Mistake 1

“I should start by self-hosting.”

### Better

Start with hosted APIs unless you have a strong reason not to.

---

### Mistake 2

“I should optimize cost before I even know if the product works.”

### Better

First learn the required quality level.

---

### Mistake 3

“The strongest model is always the best choice.”

### Better

Best depends on:

- task complexity
- speed needs
- cost constraints
- context needs

---

### Mistake 4

“Reasoning models are automatically better in every case.”

### Better

They are useful for some tasks, slower and more expensive, and require more context/examples.

---

### Mistake 5

“Context window size is a small detail.”

### Better

Context size can fundamentally change what kind of prototype you can build.

---

## 24) The chapter as a decision tree

```text
Need a model?
   │
   ├─ Are you still prototyping?
   │      └─ Yes → use hosted APIs
   │
   ├─ Is quality the biggest concern right now?
   │      └─ Yes → start with strongest model
   │
   ├─ Is the task document/code heavy?
   │      └─ Yes → prioritize large context window
   │
   ├─ Is the task deep synthesis/analysis?
   │      └─ Yes → consider reasoning model
   │
   └─ Is this high-volume + latency-sensitive?
          └─ Yes → test cheaper/faster smaller model
```

---

## 25) What the chapter is _not_ trying to teach

This chapter is not a full infrastructure chapter.
It does not deeply teach:

- GPU serving
- quantization
- inference stacks
- fine-tuning pipelines
- benchmark methodology
- custom deployment architecture

Its job is simpler:

> give you the mental model for making early smart choices

---

## 26) Best concise explanation of Chapter 2

If you had to explain the whole chapter in 30 seconds:

> Most builders should start with hosted model APIs, because early product development is about speed of iteration, not infrastructure control. Model choice is mainly a tradeoff between accuracy, cost, and latency. Context window size can dramatically affect what kinds of products are easy to build, and reasoning models are useful for slower, more complex tasks that need lots of context and examples. Start with the best model that proves the product works, then optimize later.

---

## 27) Final review sheet

### Must-remember facts

- Start with hosted APIs.
- Bigger models usually mean better quality, higher cost, and slower speed.
- Context window size is a major product design factor.
- Reasoning models are slower and need more context/examples.
- Model choice should stay flexible over time.

### Must-remember mental model

```text
Prototype with the strongest practical hosted model first.
Only optimize for cost and latency after you understand the quality bar.
```

### Must-remember builder lesson

The hardest mistake in early AI product work is optimizing the wrong thing too soon.

Chapter 2 teaches you to optimize in this order:

```text
Correctness → product fit → cost → latency → infrastructure
```

That is why this chapter is so valuable.
