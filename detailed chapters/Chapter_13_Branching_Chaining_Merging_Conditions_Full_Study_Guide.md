# Chapter 13 — Branching, Chaining, Merging, Conditions
*Complete study guide in markdown, designed for memory, clarity, and fast review*

> Goal: explain **every concept in Chapter 13** clearly and memorably using diagrams, tables, examples, and practical workflow design intuition.

---

## 1) Summary of the chapter
Chapter 13 teaches the **core control-flow primitives** of workflow graphs.

It covers five main ideas:

1. **Branching** lets you split work into multiple paths.
2. **Chaining** lets one step feed into the next in order.
3. **Merging** lets separate paths converge and combine results.
4. **Conditions** let later steps run only if earlier results meet some rule.
5. Good workflow design means steps should have:
   - meaningful inputs/outputs
   - narrow scope
   - ideally only one LLM job per step

That is the whole chapter in one line:

```text
Workflow graphs are built from a few simple control-flow primitives that you combine to get reliability.
```

---

## 2) The chapter in one figure

```text
Start
  ↓
Step A
  ├─ Branch 1 ─┐
  ├─ Branch 2 ─┼─► Merge ─► Conditional Step ─► End
  └─ Branch 3 ─┘
```

That is almost the whole chapter in one picture.

---

## 3) Why this chapter exists
Chapter 12 said:
- free-form agents can have too much freedom
- workflows add predictability

Chapter 13 answers the next question:

> “Okay, but how do I actually build a workflow graph?”

Its answer is:
- learn a small set of primitives
- combine them cleanly
- do not overcomplicate the graph

So this chapter is the **grammar of workflow design**.

---

## 4) The four primitives
The chapter is structured around four main primitives:

| Primitive | What it does |
|---|---|
| Branching | split work into multiple paths |
| Chaining | run steps in sequence |
| Merging | combine paths/results back together |
| Conditions | decide whether a step should run |

These are the workflow “Lego bricks.”

### Memory hook
```text
Branch = split
Chain = sequence
Merge = recombine
Condition = gate
```

---

## 5) Core idea #1: Branching
The chapter opens with **branching**.

### What branching means
Branching is when one workflow path splits into multiple paths.

### Why use branching?
The chapter gives a very clear use case:

> trigger multiple LLM calls on the same input

### Example from the chapter
Suppose you have a long medical record and want to check for 12 symptoms:
- drowsiness
- nausea
- and so on

You *could* ask one LLM call to check all 12 symptoms.
But the chapter says:
> that is a lot to ask

A better design is:
- 12 parallel LLM calls
- each one checks one symptom

That is branching.

---

## 6) Figure: branching on one input

```text
Medical record
     │
     ▼
 ┌────┬────┬────┬────┐
 ▼    ▼    ▼    ▼    ▼
S1   S2   S3   S4   ...
check drowsiness
check nausea
check fatigue
check fever
...
```

### Why this helps
Instead of one overloaded model call doing a huge mixed task, you get:
- narrower steps
- cleaner responsibilities
- easier inspection
- easier debugging

### Memory hook
```text
One giant check → hard to reason about
Many narrow checks → easier to trust
```

---

## 7) Why branching is powerful
Branching is good when:
- the same input needs several different analyses
- tasks can happen independently
- you want to parallelize for speed
- each branch has a narrow purpose

### Real-life examples
#### Example 1: support ticket analysis
One ticket can branch into:
- urgency detection
- sentiment analysis
- product area classification
- escalation check

#### Example 2: document analysis
One document can branch into:
- summary generation
- risk extraction
- entity extraction
- compliance flagging

Each branch does one job.

---

## 8) Code concept: branching in Mastra
The chapter says:

> In Mastra, you create branches with the `.step()` command.

The book’s actual code image is not fully machine-readable in the uploaded PDF, but the important concept is:

- define multiple steps
- connect them so they run off the same upstream data

### Conceptual pseudo-code
```ts
workflow
  .step(checkDrowsiness)
  .step(checkNausea)
  .step(checkFatigue)
```

### What matters more than syntax
The deeper lesson is:
- branching is not random duplication
- branching is targeted decomposition

---

## 9) Core idea #2: Chaining
The chapter then explains **chaining**.

### What chaining means
Chaining is the simplest workflow operation:
- one step runs
- then the next step uses its result
- then the next step uses *that* result

### Chapter example
Sometimes you want to:
- fetch data from a remote source
- then feed it into an LLM

Or:
- run one LLM call
- then pass its output into another LLM call

That is chaining.

---

## 10) Figure: chaining

```text
Fetch data
   ↓
Process data
   ↓
Generate answer
   ↓
Format result
```

This is the simplest workflow shape.

### Memory hook
```text
Branching = same input to many paths
Chaining = one output becomes next input
```

---

## 11) Why chaining is useful
Chaining is useful when a task has a natural order.

### Examples
#### Example 1: support workflow
1. classify request
2. retrieve relevant docs
3. draft answer
4. format answer

#### Example 2: coding workflow
1. inspect codebase
2. generate plan
3. write patch
4. run checks
5. summarize result

### Why chaining helps
Each step becomes:
- narrower
- easier to test
- easier to trace
- easier to replace later

---

## 12) Chapter detail: later steps get prior context
The chapter explicitly says:

> each step in the chain waits for the previous step to complete, and has access to previous step results via context

This is a very important operational detail.

### Why it matters
A chain is not just a sequence in time.
It is also a sequence of **data dependency**.

### Simple mental model
```text
Step 2 can see Step 1's output
Step 3 can see Step 2's output
```

That is why chains are powerful.

---

## 13) Code concept: chaining in Mastra
The chapter says:

> In Mastra, you chain with the `.then()` command.

Again, the image code is not fully extractable from the uploaded PDF, but the key idea is clear.

### Conceptual pseudo-code
```ts
fetchData
  .then(processData)
  .then(generateAnswer)
```

### What matters more than exact syntax
The main lesson is:
- chaining is sequential data flow
- each step depends on the previous one

---

## 14) Core idea #3: Merging
After branching paths split apart, you often need them to come back together.

That is **merging**.

### What merging means
Merging is when separate branches converge and their results are combined.

### Why it matters
Branching without merging often leaves you with:
- separate fragments
- no final answer
- no single artifact

Merging is how you turn parallel work into one usable result.

---

## 15) Figure: merging

```text
Branch A ─┐
Branch B ─┼─► Merge results ─► Next step
Branch C ─┘
```

### Plain-English explanation
Merging is the “bring it all back together” step.

### Memory hook
```text
Branching creates pieces.
Merging reassembles the puzzle.
```

---

## 16) Why merging is important
Suppose you branched a document review into:
- legal risk
- financial risk
- product risk

Eventually, someone or something has to combine those findings into:
- one report
- one score
- one decision
- one user-facing answer

That convergence is merging.

### Real-life examples
#### Example 1: product launch analysis
Parallel branches:
- marketing review
- legal review
- engineering readiness

Merge:
- unified launch readiness summary

#### Example 2: coding assistant
Parallel branches:
- lint results
- test results
- typecheck results

Merge:
- one final status report

---

## 17) Core idea #4: Conditions
The chapter then covers **conditions**.

### What conditions mean
Sometimes a step should only run if some earlier result meets a rule.

### Chapter wording
The workflow needs to make decisions based on intermediate results.

That is what conditions are for.

### Example
- if fetch succeeded → process data
- if fetch failed → skip process data or go to fallback
- if risk is high → escalate
- if confidence is low → ask human

Conditions are execution gates.

---

## 18) Figure: condition as a gate

```text
Fetch data
   ↓
Did fetch succeed?
   ├─ Yes → Process data
   └─ No  → Error path / fallback
```

### Memory hook
```text
Condition = "should this next step run?"
```

---

## 19) Important chapter nuance: child-step conditions
The chapter says something subtle but important:

> in workflow graphs, because multiple paths can typically execute in parallel, in Mastra we define the conditional path execution on the child step rather than the parent step

### Why that matters
In ordinary imperative programming, you might think:
```text
parent decides what child happens
```

But in graph workflows, the model is:
- multiple possible next nodes exist
- each child node can declare whether it should run

### Plain-English explanation
Instead of the parent step saying:
> “Only go here”

the child step says:
> “I run only if condition X is true”

That is a graph-native way to think.

---

## 20) Chapter example for conditions
The chapter gives an example where:
- `processData` runs
- but only if `fetchData` succeeded

That is a very clean condition pattern.

### Conceptual pseudo-code
```ts
processData.runIf(fetchData.status === "success")
```

This is not claimed as the exact Mastra API, but it captures the meaning.

### Builder lesson
Conditions let you make the graph responsive to reality instead of blindly following a fixed line.

---

## 21) Best practices and notes
The chapter ends with **Best Practices and Notes**.

This section is very important because it turns graph primitives into engineering advice.

The chapter gives three especially important best practices:

1. Each step’s input/output should be **meaningful**
2. Each step should usually ask the LLM to do **one thing at a time**
3. More advanced workflow patterns like loops and retries can be built by **combining these primitives**

Let’s unpack those.

---

## 22) Best practice #1: meaningful step I/O
The chapter says it is helpful to compose steps so that the input/output at each step is meaningful, because you will see it in tracing.

### Why this matters
If step outputs are vague, tracing becomes ugly and confusing.

### Bad step output
```text
"done"
```

### Better step output
```json
{
  "symptom": "nausea",
  "present": true,
  "evidence": "patient reported nausea after treatment"
}
```

### Builder lesson
A step should produce something a human can understand in a trace.

### Memory hook
```text
If a trace step is meaningless, the step design is weak.
```

---

## 23) Best practice #2: one LLM job per step
The chapter says:

> decompose steps in such a way that the LLM only has to do one thing at one time. This usually means no more than one LLM call in any step.

This is one of the best practical lessons in the entire workflow section.

### Why this matters
If one step tries to:
- classify
- summarize
- decide
- format
- and extract entities

all at once, then:
- it becomes hard to debug
- hard to evaluate
- hard to trust

### Better design
One step = one cognitive job.

### Figure

```text
Weak step:
  classify + summarize + choose next action + reformat

Better:
  Step 1 = classify
  Step 2 = summarize
  Step 3 = choose next action
  Step 4 = format
```

### Memory hook
```text
One step, one brain move.
```

---

## 24) Best practice #3: compose primitives into advanced behavior
The chapter says:

> loops, retries, and other special cases can be made by combining these primitives

This is a major insight.

### What it means
You do not need a huge separate theory for every workflow feature.
Many advanced behaviors can be built from:
- branch
- chain
- merge
- condition

### Examples
#### Retry
- condition on failure
- loop back to earlier step

#### Review cycle
- branch to reviewer
- merge feedback
- continue chain

#### Human checkpoint
- condition triggers pause path
- later resume continues chain

This is how simple primitives become powerful systems.

---

## 25) Figure: advanced behaviors from simple primitives

```text
Primitives:
  branch + chain + merge + condition
        ↓
Can build:
  retries
  loops
  fallback paths
  review stages
  human checkpoints
```

This is one of the chapter’s deepest lessons.

---

## 26) Real-life examples that make Chapter 13 stick

### Example 1: medical record symptom review
Input:
- one long medical record

Branch:
- one symptom checker per symptom

Merge:
- final symptom table

This is the chapter’s most direct example.

---

### Example 2: support request pipeline
1. chain: classify request
2. branch: urgency, sentiment, account lookup in parallel
3. merge: combine analysis
4. condition: if high risk, escalate

This uses all four primitives together.

---

### Example 3: coding workflow
1. chain: inspect code → plan changes
2. branch: run lint / tests / typecheck in parallel
3. merge: final build-health summary
4. condition: if any failed, go to repair step

This is a very memorable modern use case.

---

## 27) Tiny code examples that make the chapter memorable

### Example A: branching
```python
def check_drowsiness(record): ...
def check_nausea(record): ...
def check_fever(record): ...

results = {
    "drowsiness": check_drowsiness(record),
    "nausea": check_nausea(record),
    "fever": check_fever(record),
}
```

This shows the idea of one input spawning many narrow checks.

---

### Example B: chaining
```python
data = fetch_data()
processed = process_data(data)
answer = generate_answer(processed)
```

This is the simplest possible chain.

---

### Example C: merging
```python
merged = {
    "lint": lint_result,
    "tests": test_result,
    "typecheck": typecheck_result
}
```

This is the essence of merge: put parallel outputs back into one artifact.

---

### Example D: condition
```python
if fetch_success:
    result = process_data(data)
else:
    result = {"error": "fetch failed"}
```

This is the essence of conditional execution.

---

## 28) Common beginner mistakes this chapter prevents

### Mistake 1
“One giant LLM call is simpler, so it must be better.”

### Better
Often better to break the work into smaller graph steps.

---

### Mistake 2
“Branching is always good.”

### Better
Branch only when tasks are truly separable and independently useful.

---

### Mistake 3
“A step can do many LLM tasks at once.”

### Better
Usually keep each step to one main model job.

---

### Mistake 4
“Tracing is something I add later.”

### Better
Design step inputs/outputs to be trace-friendly from the start.

---

### Mistake 5
“Loops, retries, and complex flows need totally different ideas.”

### Better
Many of them come from recombining the same small primitives.

---

## 29) The chapter as a decision tree

```text
Need to build a workflow graph?
   │
   ├─ Need multiple analyses from same input?
   │      └─ Branch
   │
   ├─ Need ordered dependency between steps?
   │      └─ Chain
   │
   ├─ Need separate paths combined later?
   │      └─ Merge
   │
   ├─ Need execution to depend on prior result?
   │      └─ Condition
   │
   └─ Want reliability?
          └─ keep step I/O meaningful and one LLM task per step
```

---

## 30) Table: all major concepts in Chapter 13

| Concept | Meaning | Why it matters |
|---|---|---|
| branching | split one path into multiple paths | parallelize and narrow tasks |
| chaining | run steps in sequence | preserve order and dependency |
| merging | combine outputs from separate paths | create one usable result |
| conditions | gate step execution on prior results | responsive control flow |
| child-step condition | condition lives on the step that may run | graph-native execution logic |
| meaningful step I/O | each step output should be interpretable | better tracing and debugging |
| one LLM call per step | narrow step scope | reliability and clarity |
| advanced patterns from primitives | loops/retries/fallbacks built by composition | keeps workflow design simple |

---

## 31) What Chapter 13 is *not* trying to do
This chapter is not yet fully about:
- suspend/resume mechanics
- streaming updates
- observability UI
- multi-agent orchestration
- eval design

It is doing the workflow-core job:

> teaching the four basic control-flow operations you will keep reusing everywhere

That is the real mission of Chapter 13.

---

## 32) Best concise explanation of Chapter 13
If you had to explain the whole chapter in 30 seconds:

> Chapter 13 explains the basic control-flow primitives of workflow graphs: branching, chaining, merging, and conditions. Branching splits work into multiple paths, chaining runs steps in order, merging recombines paths, and conditions decide whether a step should run based on earlier results. The chapter’s main best practices are to keep each step meaningful in traces and usually limit each step to one LLM task at a time.

---

## 33) Final review sheet

### Must-remember facts
- Branching can trigger multiple LLM calls on the same input.
- Chaining is the simplest sequential workflow pattern.
- Merging combines diverged paths back together.
- Conditions gate later steps using intermediate results.
- In graph workflows, conditions often live on the child step.
- Step inputs/outputs should be meaningful for tracing.
- Usually keep one LLM call per step.
- Loops/retries can be built by combining these primitives.

### Must-remember mental model
```text
Branch = split
Chain = order
Merge = recombine
Condition = gate
```

### Must-remember builder lesson
When a workflow feels messy, do not immediately invent a new abstraction.

First ask:
- is this a branch?
- a chain?
- a merge?
- or a condition?

Most workflow design is just composing these few primitives well.

That is the deep practical lesson of Chapter 13.
