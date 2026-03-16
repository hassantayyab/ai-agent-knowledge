# Chapter 27 — Evals 101

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 27** clearly and memorably using diagrams, tables, examples, and practical engineering framing.

---

## 1) Summary of the chapter

Chapter 27 introduces **evals** as the basic discipline for measuring AI agent quality.

The chapter teaches these core ideas:

1. Traditional software tests usually have clear pass/fail conditions, but AI outputs are **non-deterministic**, so the same input may produce different outputs across runs. fileciteturn28file1L116-L119
2. Evals help bridge that gap by giving you **quantifiable metrics** for agent quality. fileciteturn28file1L116-L119
3. Instead of binary pass/fail, evals usually return **scores between 0 and 1**. fileciteturn28file1L116-L119
4. You should think of evals more like:
   - performance testing
   - quality monitoring
   - trend tracking over time
     than like ordinary strict unit tests. fileciteturn28file1L116-L119
5. When writing evals, you must be explicit about **what exactly you want to test**. fileciteturn28file1L116-L119
6. There are different eval levels, analogous to software tests:
   - more local / narrow checks
   - more end-to-end / broad checks fileciteturn28file1L118-L119
7. In structured systems like RAG pipelines or workflows, you often want to evaluate:
   - each step
   - and then the whole system end-to-end fileciteturn28file1L118-L119

That is the whole chapter in one line:

```text
Evals are how you measure AI quality in a world where outputs vary, using scores and trend signals instead of only binary pass/fail tests.
```

---

## 2) The chapter in one figure

```text
Prompt / input
      ↓
Agent or workflow runs
      ↓
Output is produced
      ↓
Eval checks quality
      ↓
Score from 0 to 1
      ↓
Track over time, compare variants, improve system
```

This is the chapter’s entire shape.

---

## 3) Why this chapter matters

Once you build an agent, the next hard question is:

> “How do I know whether it is actually good?”

In normal software, many tests look like this:

```text
Expected result == Actual result
```

But AI systems often do not work that way.

### Why?

Because LLM outputs can vary while still being:

- correct
- helpful
- acceptable
- policy-compliant

That makes evaluation harder and more interesting.

### Memory hook

```text
AI quality often needs measuring, not just matching.
```

---

## 4) Core idea #1: AI outputs are non-deterministic

The chapter opens with the biggest difference between ordinary software testing and AI testing:

> AI outputs are non-deterministic — they can vary with the same input. fileciteturn28file1L116-L119

### What that means

If you run the same AI task multiple times, you may get:

- different wording
- different ordering
- different examples
- slightly different levels of completeness

Sometimes those differences are harmless.
Sometimes they matter a lot.

### Why this matters

A strict pass/fail test may wrongly fail a perfectly good answer, or miss subtle quality drift.

### Memory hook

```text
Same input does not guarantee same output.
```

That is the foundation of the whole evals chapter.

---

## 5) Why ordinary pass/fail tests are not enough

Traditional tests are still useful, but they are not sufficient for many AI systems.

### Example

Prompt:

> “Summarize this security policy for an employee.”

Two different answers might both be acceptable, even if they are not identical.

### Problem

A classic exact-match test might say:

- answer A = pass
- answer B = fail

even though both are fine.

### Builder lesson

For AI, correctness is often a **range** or **score**, not only a yes/no.

---

## 6) Core idea #2: evals bridge the gap

The chapter says:

> Evals help bridge this gap by providing quantifiable metrics for measuring agent quality. fileciteturn28file1L116-L119

### What “bridge the gap” means

Evals sit between:

- pure human intuition
  and
- rigid pass/fail tests

They give you a middle ground:

- structured
- measurable
- comparable
- automatable enough to be useful

### Plain-English explanation

Evals are the instrumentation for AI quality.

### Memory hook

```text
Evals make fuzzy output quality measurable enough to improve.
```

---

## 7) Core idea #3: eval scores are usually continuous

The chapter says:

> Instead of binary pass/fail results, evals return scores between 0 and 1. fileciteturn28file1L116-L119

### Why this matters

A score lets you say:

- this answer was pretty good
- this one was weak
- quality improved overall
- version B scored higher than version A

instead of only:

- pass
- fail

### Example

| Output              | Score |
| ------------------- | ----- |
| Great answer        | 0.93  |
| Good but incomplete | 0.74  |
| Weak answer         | 0.31  |

### Memory hook

```text
Evals often give gradients, not only gates.
```

That is one of the most important ideas in the chapter.

---

## 8) Why scores are better than binary for AI quality

Scores are useful because they let you track:

- trend over time
- regression size
- improvement magnitude
- comparison across prompts/models

### Example

You change:

- model
- prompt
- retrieval strategy
- tool descriptions

and then compare mean eval score before vs after.

That gives you a much richer signal than only “some tests passed.”

---

## 9) Core idea #4: think of evals like performance testing

The chapter gives a very useful analogy:

> Think about evals sort of like including, say, performance testing in your CI pipeline. There’s going to be some randomness in each result, but on the whole and over time there should be a correlation between application performance and test results. fileciteturn28file1L116-L119

This is one of the best lines in the chapter.

### Why this analogy helps

Performance tests can fluctuate slightly too:

- network jitter
- machine noise
- background load

But over time, they still tell you something real.

The same is true for evals.

### Memory hook

```text
One eval run may wobble.
A series of eval runs should still show signal.
```

---

## 10) Figure: noisy single runs vs stable trend

```text
Single run:
  maybe noisy

Many runs over time:
  trend becomes visible
```

### Builder lesson

Do not overreact to a single eval number.
Look for meaningful movement across runs and datasets.

---

## 11) Core idea #5: define what you want to test

The chapter says:

> When writing evals, it’s important to think about what exactly you want to test. fileciteturn28file1L116-L119

This is the chapter’s strongest practical instruction.

### Why this matters

“Is my agent good?” is too vague.

You should ask narrower questions like:

- does it answer correctly?
- does it use retrieved context properly?
- does it follow the workflow?
- does it complete the task?
- does it avoid hallucinating?
- does it call the right tool?
- does it produce the required format?

### Memory hook

```text
Bad evals ask vague questions.
Good evals test specific qualities.
```

---

## 12) Good eval design starts with explicit dimensions

A strong eval usually targets a specific property.

### Examples

| Eval target      | Question                                            |
| ---------------- | --------------------------------------------------- |
| Correctness      | Is the answer factually right?                      |
| Faithfulness     | Does it stay grounded in the given context?         |
| Completeness     | Does it cover the important points?                 |
| Relevance        | Does it answer the user’s question?                 |
| Tool usage       | Did it call the correct tool and use it properly?   |
| Workflow quality | Did it complete the right steps in the right order? |

### Why this matters

If you do not define the dimension, the score becomes hard to trust or improve against.

---

## 13) Core idea #6: there are different kinds of evals, just like tests

The chapter says:

> There are different kinds of evals just like there are different kinds of tests. fileciteturn28file1L118-L119

This is a very important framing.

### Why it matters

You should not expect one eval to tell you everything.

Instead, you often need a **portfolio** of evals.

### Analogy to software

- unit test checks one small thing
- integration test checks interaction
- end-to-end test checks full system behavior

AI evals often need a similar layered structure.

### Memory hook

```text
There is no one eval to rule them all.
```

---

## 14) Unit-like evals vs end-to-end evals

The chapter makes the analogy explicit:

> Unit tests are easy to write and run but might not capture the behavior that matters; end-to-end tests might capture the right behavior but they might be more flaky. fileciteturn28file1L118-L119

This is a deep point.

### Unit-like evals

These are:

- narrow
- easier to debug
- faster to run
- easier to isolate

### End-to-end evals

These are:

- closer to real user experience
- more holistic
- often more realistic
- but noisier and harder to debug

### Tradeoff table

| Eval style | Strength                    | Weakness                      |
| ---------- | --------------------------- | ----------------------------- |
| Unit-like  | precise, easier to diagnose | may miss system-level failure |
| End-to-end | realistic, holistic         | more brittle or noisy         |

### Memory hook

```text
Narrow evals explain.
End-to-end evals validate.
```

---

## 15) Why both eval levels matter

If you only run end-to-end evals:

- you know something is wrong
- but not necessarily where

If you only run step-level evals:

- each step may look okay
- but the total system may still fail

### Builder lesson

Good eval practice usually mixes both:

- local evals for diagnosis
- end-to-end evals for reality

---

## 16) Core idea #7: structured systems should be evaluated step-by-step

The chapter says:

> if you’re building a RAG pipeline, or a structured workflow, you may want to test each step along the way, and then after that test the behavior of the system as a whole. fileciteturn28file1L118-L119

This is one of the strongest engineering lessons in the chapter.

### Why this matters

Because pipelines fail in stages.

For example, a RAG system may fail because:

- chunking is weak
- retrieval is poor
- reranking is bad
- synthesis hallucinated

A workflow may fail because:

- wrong branch chosen
- tool step failed
- handoff was weak
- final formatting broke

### Memory hook

```text
If the system has stages, the eval strategy should have stages too.
```

---

## 17) Figure: stage evals + end-to-end eval

```text
Pipeline
  ├─ Step 1 eval
  ├─ Step 2 eval
  ├─ Step 3 eval
  └─ End-to-end eval
```

This is the chapter’s most useful operational picture.

---

## 18) Example: RAG eval decomposition

Suppose you build a support assistant with RAG.

You may want to evaluate:

1. retrieval quality
2. faithfulness to retrieved context
3. final answer relevance
4. end-to-end customer usefulness

### Why this is useful

If the final answer is bad, you can locate whether the problem came from:

- retrieval
- generation
- both

That is exactly the chapter’s step-wise eval lesson.

---

## 19) Example: workflow eval decomposition

Suppose you build an approval workflow.

You may want to evaluate:

1. classification of request type
2. policy-check correctness
3. escalation decision
4. final recommendation quality
5. end-to-end workflow completion

### Why this matters

Again, it turns “the workflow feels wrong” into measurable sub-problems.

---

## 20) Evals are part of iteration, not just auditing

A major hidden lesson in the chapter is that evals are not only for proving quality after the fact.

They are for:

- iterating
- comparing versions
- tuning prompts
- tuning tools
- tuning retrieval
- spotting regressions

### Plain-English explanation

Evals are the steering wheel for improvement.

### Memory hook

```text
Without evals, iteration becomes guesswork.
```

---

## 21) Real-life examples that make Chapter 27 stick

### Example 1: answer quality over time

You change your prompt.
If the average eval score rises from:

- 0.68 to 0.79

that is a meaningful signal, even if individual outputs vary.

---

### Example 2: RAG pipeline

You test:

- retrieval score
- faithfulness score
- final answer score

The final score falls.
The retrieval score stayed high.
The faithfulness score dropped.

Now you know the likely problem is synthesis, not search.

---

### Example 3: workflow agent

A workflow completes the task only 72% of the time.
Step-level evals show one branch-selection step is weak.

Now you can fix the branch selection instead of guessing.

---

### Example 4: model comparison

Two models answer the same eval dataset.

| Model   | Mean score |
| ------- | ---------- |
| Model A | 0.74       |
| Model B | 0.82       |

That is a clean signal for experimentation.

---

## 22) Tiny code examples that make the chapter memorable

### Example A: simple scored eval

```python
def score_answer(answer):
    # toy example: real evals are richer
    if "refund" in answer.lower():
        return 0.8
    return 0.3
```

This is overly simple, but it helps anchor the idea that evals often return scores, not just pass/fail.

---

### Example B: end-to-end eval record

```python
eval_result = {
    "input": "How do duplicate charge refunds work?",
    "output": "You can request a refund for duplicate charges within 30 days.",
    "score": 0.91
}
```

This makes the score-based idea concrete.

---

### Example C: step-level evals in a pipeline

```python
pipeline_eval = {
    "retrieval_score": 0.88,
    "faithfulness_score": 0.92,
    "final_answer_score": 0.74
}
```

This shows how you can evaluate stages separately.

---

### Example D: compare trends over time

```python
weekly_scores = [0.71, 0.73, 0.76, 0.79]
```

This captures the chapter’s “look for trend, not one noisy result” idea.

---

## 23) Common beginner mistakes this chapter prevents

### Mistake 1

“AI testing should work like exact-match unit tests.”

### Better

Many AI tasks need score-based evaluation because outputs vary.

---

### Mistake 2

“One eval score tells me everything.”

### Better

You usually need multiple evals and multiple runs.

---

### Mistake 3

“If the output changes slightly, the model is broken.”

### Better

Variation is normal. The question is whether quality trends remain good.

---

### Mistake 4

“End-to-end evals are enough.”

### Better

Step-level evals are also important for diagnosis.

---

### Mistake 5

“Evals are only for final QA.”

### Better

Evals are part of everyday iteration and tuning.

---

## 24) The chapter as a decision tree

```text
Need to evaluate an AI system?
   │
   ├─ Is the task deterministic and exact-matchable?
   │      └─ Yes → classic pass/fail tests may help
   │
   └─ No / partly
        │
        ├─ Need a measurable quality signal?
        │      └─ use eval scores
        │
        ├─ Is the system a pipeline or workflow?
        │      └─ eval steps individually
        │
        ├─ Need real-user realism?
        │      └─ add end-to-end evals
        │
        └─ Need iteration over time?
               └─ track scores and trends across runs
```

This is the most operational summary of Chapter 27.

---

## 25) Table: all major concepts in Chapter 27

| Concept                  | Meaning                                   | Why it matters                         |
| ------------------------ | ----------------------------------------- | -------------------------------------- |
| eval                     | measurable quality check for AI behavior  | core AI iteration tool                 |
| non-deterministic output | same input may produce varied outputs     | why rigid testing is insufficient      |
| score between 0 and 1    | graded signal instead of binary pass/fail | supports comparison and trend tracking |
| quantifiable metric      | numeric quality measure                   | helps automate improvement             |
| unit-like eval           | narrow step or component evaluation       | easier diagnosis                       |
| end-to-end eval          | full system behavior check                | closer to real experience              |
| step-level testing       | evaluate each pipeline/workflow stage     | localizes failures                     |
| trend tracking           | compare scores over time                  | spots regressions and improvements     |
| eval target definition   | specify exactly what is being measured    | prevents vague, weak evals             |

---

## 26) What Chapter 27 is _not_ trying to do

This chapter is not yet deeply teaching:

- textual eval rubrics in detail
- LLM-as-judge prompting
- classification evals
- tool-usage evals
- A/B testing
- human review processes

Those are covered in later eval chapters.

This chapter is doing something more foundational:

> explaining what evals are, why AI systems need them, and how to think about them as graded, layered measurements rather than only binary tests

That is its real mission.

---

## 27) Best concise explanation of Chapter 27

If you had to explain the whole chapter in 30 seconds:

> Chapter 27 introduces evals as the way to measure AI system quality when outputs are non-deterministic and cannot always be tested with exact-match assertions. Instead of binary pass/fail, evals often return scores between 0 and 1. The chapter explains that you should decide explicitly what you want to test, that different evals correspond to different levels like unit tests versus end-to-end tests, and that systems such as RAG pipelines or workflows should often be evaluated both step-by-step and as a whole.

---

## 28) Final review sheet

### Must-remember facts

- AI outputs are non-deterministic, so the same input can produce different outputs. fileciteturn28file1L116-L119
- Evals help bridge the gap by providing quantifiable metrics for quality. fileciteturn28file1L116-L119
- Evals often return scores between 0 and 1 instead of binary pass/fail. fileciteturn28file1L116-L119
- Evals are like performance testing in CI: noisy per run, but useful over time. fileciteturn28file1L116-L119
- You must think carefully about what exactly you want to test. fileciteturn28file1L116-L119
- There are different kinds of evals, analogous to different kinds of software tests. fileciteturn28file1L118-L119
- For structured workflows or RAG pipelines, you often want step-level evals plus end-to-end evals. fileciteturn28file1L118-L119

### Must-remember mental model

```text
Evals are graded measurements for AI quality, not just binary correctness checks.
```

### Must-remember builder lesson

Do not ask only:

> “Did it pass?”

Ask:

> “How well did it do, which part failed, and is quality trending up or down?”

That is the deep practical lesson of Chapter 27.
