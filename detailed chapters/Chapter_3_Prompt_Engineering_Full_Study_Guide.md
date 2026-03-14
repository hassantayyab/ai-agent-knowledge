# Chapter 3 — Prompt Engineering

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 3** in a compact but memorable way using diagrams, tables, examples, and tiny code demos.

---

## 1) Summary of the chapter

Chapter 3 teaches the **fundamentals of prompt engineering**.

It covers these main ideas:

1. Prompting is one of the most important practical skills in working with LLMs.
2. There are three common prompting styles:
   - **zero-shot**
   - **single-shot**
   - **few-shot**
3. More examples usually improve control, but they also cost more tokens and time.
4. A powerful trick is to let the model help you write the prompt itself.
5. **System prompts** shape model behavior, especially tone and role.
6. Prompt formatting matters:
   - capitalization
   - XML-like tags
   - explicit sections
7. Real-world prompts are often much longer and more detailed than beginners expect.

That is the whole chapter in one line:

```text
Clear instructions + examples + structure + iteration = better LLM behavior
```

---

## 2) The chapter in one figure

```text
          Need better model behavior?
                    │
                    ▼
         Write a clearer prompt
                    │
                    ▼
    Add examples if needed (shot prompting)
                    │
                    ▼
   Add structure (sections / tags / constraints)
                    │
                    ▼
        Test → refine → test again
```

---

## 3) Core idea #1: Prompting is a core skill

The chapter says prompt engineering is one of the most important skills for builders working with LLMs.

### Why?

Because LLMs are highly sensitive to:

- how a task is described
- what examples are given
- how the output is requested
- how constraints are framed

### Beginner mistake

A lot of people think:

> “The model is smart, so vague instructions should be enough.”

The chapter is teaching the opposite:

> The better your instructions, the better your chances of getting useful behavior.

### Real-life analogy

Prompting is like giving work to a very capable new employee:

- vague request → messy output
- clear request → much better result

---

## 4) Prompt engineering is not magic

The chapter does **not** present prompting as mystical word sorcery.
It treats prompting more like:

- specification writing
- task design
- interface design

### Better mental model

```text
Prompt = task specification for the model
```

That is a much healthier way to think about it than:

```text
Prompt = secret spell
```

---

## 5) Core idea #2: zero-shot prompting

### Definition

**Zero-shot prompting** means:

- you ask the model to do something
- without giving any example first

### Example

```text
Translate this into French:
"Hello, how are you?"
```

No examples are given.  
You just ask.

### Why it is useful

- fastest to write
- cheapest in tokens
- often good enough for simple tasks

### Why it can fail

- output format may be inconsistent
- style may not match what you want
- complex tasks may drift

### Tiny example

```python
prompt = "Summarize this paragraph in one sentence."
```

That is zero-shot.

---

## 6) Core idea #3: single-shot prompting

### Definition

**Single-shot prompting** means:

- you give **one example**
- then ask the model to do the real task

### Example

```text
Example:
Input: "The weather is sunny."
Output: "It is a sunny day."

Now do this:
Input: "The meeting was delayed by two hours."
Output:
```

### Why it helps

It gives the model:

- a pattern
- a style
- a target format

### When to use it

Use it when:

- zero-shot is close but inconsistent
- you want a particular output shape
- one example is enough to anchor behavior

---

## 7) Core idea #4: few-shot prompting

### Definition

**Few-shot prompting** means:

- you give **multiple examples**
- then ask the model to do the real task

### Example

```text
Example 1:
Input: "Cats are playful."
Output: "Cats often behave in playful ways."

Example 2:
Input: "Dogs are loyal."
Output: "Dogs are commonly known for loyalty."

Now do this:
Input: "Birds can migrate long distances."
Output:
```

### Why it works

Few-shot prompting helps the model infer:

- the pattern
- the tone
- the formatting rule
- edge-case behavior

### Key lesson from the chapter

More examples often improve control.

But that comes with tradeoffs.

---

## 8) The tradeoff of few-shot prompting

The chapter clearly explains that while examples are useful, they cost:

- **more tokens**
- **more prompt length**
- **more latency**
- **more money**
- **more setup effort**

### Table: prompt styles compared

| Style       | Examples given | Pros                         | Cons                                    |
| ----------- | -------------- | ---------------------------- | --------------------------------------- |
| Zero-shot   | 0              | fastest, cheapest            | least controlled                        |
| Single-shot | 1              | good balance                 | may still be too weak for complex tasks |
| Few-shot    | several        | best control and consistency | longest, most expensive                 |

### Practical lesson

Use the **smallest number of examples** that reliably produces the behavior you want.

That is the chapter’s practical wisdom.

---

## 9) Figure: prompt control increases with examples

```text
Zero-shot   → fast, low control
Single-shot → balanced
Few-shot    → slower, higher control
```

Or as a simple ladder:

```text
More examples
      ▲
      │   more control
      │   more consistency
      │   more cost
      │
      └──────────────►
```

---

## 10) Core idea #5: let the model write the prompt

This is one of the smartest practical tips in the chapter.

### The idea

If you are stuck, ask the model:

- to draft the prompt
- to improve your prompt
- to critique your prompt

### Example

```text
Generate a prompt for an AI assistant that summarizes meeting notes into bullet points for executives.
```

Then after it writes one, ask:

```text
Improve this prompt for clarity, structure, and output consistency.
```

### Why this works

The model often “knows” what instructions help it behave better, especially for common tasks.

### Builder lesson

Prompt writing can itself be an iterative collaboration with the model.

---

## 11) Why this trick is powerful

Because it turns prompt engineering into a loop:

```text
Idea → draft prompt → test → ask model to improve → test again
```

That is much more practical than trying to write the perfect prompt from scratch.

### Real-life analogy

It is like asking a good editor:

> “Can you help me write the assignment instructions better?”

---

## 12) Core idea #6: system prompts matter

The chapter introduces **system prompts**.

### What a system prompt does

A system prompt tells the model:

- who it is
- how it should behave
- what tone to use
- what boundaries to respect

### Example

```text
You are a helpful assistant that explains coding concepts clearly and briefly.
```

### The chapter’s key point

System prompts are especially useful for:

- role
- tone
- behavior style
- high-level constraints

### Important nuance

The chapter says system prompts often affect **tone/persona** more than raw accuracy.

That is an important detail.

---

## 13) System prompt vs user prompt

### Simple distinction

| Prompt type   | Purpose                                         |
| ------------- | ----------------------------------------------- |
| System prompt | defines role, behavior, tone, broad constraints |
| User prompt   | asks for the actual task                        |

### Example

#### System prompt

```text
You are a patient tutor who explains ideas in simple language.
```

#### User prompt

```text
Explain what recursion is using a simple example.
```

Together, they shape the output.

---

## 14) Core idea #7: formatting matters

One of the best lessons in the chapter is that formatting is not cosmetic.
Formatting can improve reliability.

The chapter specifically mentions:

- **capitalization**
- **XML-style tags**
- **explicit structure**

### Why formatting matters

Because formatting can make instructions:

- easier for the model to separate
- easier to prioritize
- less ambiguous

---

## 15) Capitalization as emphasis

The chapter says using **CAPS** can help signal importance.

### Example

```text
You MUST return valid JSON.
DO NOT include any explanation outside the JSON.
```

### Why it helps

It acts like emphasis.
It tells the model:

> this part matters a lot

### Important caution

Don’t turn the whole prompt into shouting.
That reduces clarity.
Use emphasis selectively.

---

## 16) XML-like tags

The chapter says XML-style formatting can help.

### Example

```xml
<task>
Summarize the following article.
</task>

<constraints>
- Use 3 bullet points
- Keep each bullet under 15 words
</constraints>

<output_format>
Return markdown only.
</output_format>
```

### Why it helps

Tags can make the prompt structure explicit:

- what is task
- what is context
- what is constraint
- what is output format

### Memory hook

Tags act like labeled folders inside the prompt.

---

## 17) Explicit sections improve reliability

The chapter recommends structuring prompts into sections like:

- task
- context
- constraints
- output format

### Why this works

A model does better when the problem is separated into parts instead of dumped as one paragraph.

### Figure: weak vs strong prompt structure

```text
Weak:
"Please do this thing and here is some info and maybe respond like this..."

Strong:
[TASK]
[CONTEXT]
[CONSTRAINTS]
[OUTPUT FORMAT]
```

### Builder lesson

Good prompt structure reduces ambiguity.

---

## 18) Core idea #8: real prompts are longer than beginners expect

This is one of the most valuable reality-checks in the chapter.

The chapter includes a real-world example from **bolt.new**, a production code-generation system prompt.

### Why this matters

Beginners often imagine powerful prompts are short magic lines like:

> “Act like a senior engineer.”

But real production prompts are often:

- long
- detailed
- constraint-heavy
- environment-aware
- specific about limitations

### The chapter uses the bolt.new prompt to show:

> professional prompting often looks more like a technical spec than a clever sentence

That is a very important mindset shift.

---

## 19) What the bolt.new example is trying to teach

The exact content is less important than the lesson.

### The lesson is:

A real production prompt may contain:

- role definition
- environment description
- system limitations
- explicit prohibitions
- output expectations
- operational constraints

### Why that matters

Because real AI products live inside real environments:

- browser sandboxes
- limited runtimes
- no access to certain tools
- security constraints
- library limitations

The model must be told those things clearly.

---

## 20) Example: why environment details matter

Imagine a code assistant running in a browser-based environment where:

- `pip` does not work
- native binaries cannot run
- only standard library Python exists

If you do **not** tell the model that, it may happily suggest:

- `pip install ...`
- native compilation
- unsupported dependencies

So a long system prompt prevents invalid behavior.

### Builder lesson

A great prompt is not only about what the model should do.
It is also about what the environment allows.

---

## 21) Prompting as iterative engineering

This is the hidden backbone of the whole chapter.

The chapter is really telling you:

```text
Prompting is not one-shot writing.
Prompting is iterative engineering.
```

### The real loop

```text
Write prompt
   ↓
Test output
   ↓
Observe failure
   ↓
Refine prompt
   ↓
Test again
```

That is how prompt engineering works in real products.

---

## 22) Real-life examples that make Chapter 3 stick

### Example 1: zero-shot works

Task:

```text
Translate this sentence into German.
```

Likely fine as zero-shot.

### Example 2: single-shot helps

Task:
You want a specific style of summary.

You provide:

- one example input
- one example output

Now the model has a target.

### Example 3: few-shot is needed

Task:
You want to classify customer support tickets into a very specific internal taxonomy.

A vague prompt may drift.
A few examples make the categories clearer.

### Example 4: production prompt for coding

Task:
You are building a code assistant inside a browser sandbox.

You must tell the model:

- what language runtime exists
- what tools are unavailable
- what package limitations exist
- what output format is expected

That is exactly the kind of thing the bolt.new example demonstrates.

---

## 23) Tiny code examples to make the chapter memorable

### Example A: zero-shot prompt

```python
prompt = "Summarize this article in 2 bullet points."
```

### Example B: single-shot prompt

```python
prompt = '''
Example:
Input: "The app crashed after login."
Output: "App crash occurred after user login."

Now do this:
Input: "The payment page froze during checkout."
Output:
'''
```

### Example C: few-shot prompt

```python
prompt = '''
Example 1:
Ticket: "I was charged twice."
Category: Billing

Example 2:
Ticket: "The app won't open on Android."
Category: Technical Issue

Example 3:
Ticket: "Can I change my subscription?"
Category: Account Management

Now classify:
Ticket: "I forgot my password and can't sign in."
Category:
'''
```

### Example D: structured prompt with tags

```python
prompt = '''
<task>
Generate a release note summary.
</task>

<context>
These notes are for non-technical users.
</context>

<constraints>
- Max 5 bullets
- Plain English
</constraints>

<output_format>
Markdown bullet list
</output_format>
'''
```

---

## 24) Common beginner mistakes Chapter 3 helps prevent

### Mistake 1

“Good prompting means writing one clever sentence.”

### Better

Good prompting usually means:

- clarity
- structure
- examples
- iteration

---

### Mistake 2

“If the model fails, the model is bad.”

### Better

Sometimes the prompt is underspecified.

---

### Mistake 3

“Few-shot is always better.”

### Better

Few-shot gives more control, but costs more. Use it when needed.

---

### Mistake 4

“System prompts automatically make outputs more accurate.”

### Better

System prompts often shape role and tone more than raw correctness.

---

### Mistake 5

“Formatting is cosmetic.”

### Better

Formatting can improve instruction following and output consistency.

---

## 25) The chapter as a decision tree

```text
Need better output?
   │
   ├─ Is the task simple?
   │      └─ Yes → try zero-shot
   │
   ├─ Is output inconsistent?
   │      └─ Yes → try single-shot
   │
   ├─ Is task nuanced or format-sensitive?
   │      └─ Yes → try few-shot
   │
   ├─ Is the prompt messy?
   │      └─ Yes → add structure (task/context/constraints/output)
   │
   ├─ Still not good?
   │      └─ Ask the model to improve the prompt
   │
   └─ Is this a real product environment?
          └─ Add system prompt with environment constraints
```

---

## 26) Table: all major concepts in Chapter 3

| Concept                | Meaning                                             | Why it matters                      |
| ---------------------- | --------------------------------------------------- | ----------------------------------- |
| prompt engineering     | writing instructions for the model                  | core practical skill                |
| zero-shot              | no examples                                         | fastest, least controlled           |
| single-shot            | one example                                         | good middle ground                  |
| few-shot               | several examples                                    | highest control, more expensive     |
| prompt self-generation | ask model to write/improve prompt                   | speeds iteration                    |
| system prompt          | role/tone/behavior instructions                     | shapes assistant behavior           |
| capitalization         | emphasis for critical instructions                  | improves clarity                    |
| XML-like tags          | structured labeled sections                         | improves separation of instructions |
| explicit sections      | task/context/constraints/output                     | reduces ambiguity                   |
| production prompt      | long real-world prompt with environment constraints | closer to real AI products          |

---

## 27) What Chapter 3 is _not_ trying to do

This chapter is not trying to teach:

- prompt benchmarking science in depth
- fine-tuning
- reinforcement learning
- jailbreak defense in full depth
- formal eval systems
- advanced chain-of-thought research

Instead, it is giving you the **practical prompt toolbox** you need before building more advanced agent systems.

---

## 28) Best concise explanation of Chapter 3

If you had to explain the whole chapter in 30 seconds:

> Prompt engineering is the skill of clearly specifying what you want an LLM to do. You can prompt with no examples, one example, or several examples, depending on how much control you need. Better prompts are usually clearer, more structured, and more explicit about constraints and output format. System prompts help define model behavior, and real production prompts are often much longer and more detailed than people expect. Prompting works best as an iterative process of testing and refinement.

---

## 29) Final review sheet

### Must-remember facts

- Prompting is one of the most important LLM skills.
- Zero-shot, single-shot, and few-shot are the main prompting styles.
- More examples often improve control, but cost more.
- System prompts mainly shape role/tone/behavior.
- Formatting and structure matter.
- Real production prompts are often long and detailed.

### Must-remember mental model

```text
Prompt = task specification, not magic spell
```

### Must-remember builder lesson

When a model behaves badly, don’t only ask:

> “Is the model good enough?”

Also ask:

> “Did I specify the task clearly enough?”

That is the deep practical lesson of Chapter 3.
