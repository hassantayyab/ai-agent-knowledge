# Chapter 24 — Coding Agent Best Practices

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 24** clearly and memorably using diagrams, tables, examples, and practical engineering framing.

---

## 1) Summary of the chapter

Chapter 24 explains how to think about **coding agents** in a practical, production-minded way.

The chapter teaches these core ideas:

1. Coding agents are powerful, but they work best when their job is clearly scoped.
2. A coding agent should usually be treated like a strong but imperfect collaborator.
3. Good coding-agent systems rely on:
   - clear tasks
   - clean repository access
   - strong feedback loops
   - tests and verification
4. Planning, editing, running checks, and reviewing are often different stages that should be explicit.
5. Coding agents become more reliable when combined with:
   - workflows
   - tools
   - review loops
   - observability
6. The deeper lesson is that coding agents should operate inside a well-designed software process, not as free-roaming code gen magic.

That is the whole chapter in one line:

```text
Coding agents are most useful when they are tightly scoped, tool-assisted, test-backed, and embedded in a clear review process.
```

---

## 2) The chapter in one figure

```text
Task
  ↓
Plan change
  ↓
Inspect codebase
  ↓
Edit code
  ↓
Run checks / tests
  ↓
Review output
  ↓
Accept, revise, or escalate
```

This is the chapter’s center of gravity.

---

## 3) Why this chapter matters

By the time you reach this chapter, the book has already taught:

- tools
- workflows
- supervisors
- observability
- evaluation

Coding agents bring all of those ideas together in one especially high-stakes setting:
changing code.

### Why coding agents are different

A wrong support answer is bad.  
A wrong code change can:

- break builds
- introduce security issues
- corrupt data
- silently degrade quality
- create long debugging sessions

So the chapter is really about **discipline**.

### Memory hook

```text
Coding agents are powerful, but software punishes sloppiness.
```

---

## 4) Core idea #1: coding agents should be scoped

One of the most important lessons in any coding-agent chapter is that the task should be clear.

### Bad prompt

```text
Improve the whole codebase.
```

### Better prompt

```text
Update the login middleware so expired sessions redirect to /signin, add tests, and do not change unrelated files.
```

### Why scoping matters

A well-scoped coding task gives the agent:

- a defined goal
- a bounded search space
- clearer success criteria
- less opportunity to wander

### Memory hook

```text
The narrower the task, the safer the coding agent.
```

---

## 5) Why vague coding tasks go wrong

Vague tasks create failure modes like:

- editing too many files
- making unnecessary refactors
- misunderstanding the intended scope
- inventing architecture changes
- “fixing” things that were not broken

### Builder lesson

A coding agent needs constraints just like a human contractor does.

### Real-life analogy

If you tell a contractor:

> “Make the house better,”

you are asking for chaos.

If you say:

> “Replace the broken kitchen faucet and do not touch the plumbing behind the wall,”

you get a much safer workflow.

---

## 6) Core idea #2: coding agents need strong repository visibility

A coding agent can only work well if it can inspect the codebase properly.

### That often means access to:

- relevant files
- project structure
- tests
- configs
- docs / READMEs
- lint/typecheck/build feedback

### Why this matters

Code changes are contextual.
A good implementation often depends on:

- existing patterns
- naming conventions
- module structure
- test style
- framework usage

### Memory hook

```text
A coding agent without repo context is guessing with syntax.
```

---

## 7) Figure: coding-agent information needs

```text
Good coding agent context
  ├─ relevant source files
  ├─ tests
  ├─ configs
  ├─ docs
  ├─ build/lint signals
  └─ task constraints
```

This is the informational backbone of the chapter.

---

## 8) Core idea #3: planning should often come before editing

A recurring best practice with coding agents is:

- plan first
- edit second

### Why?

Because immediate code generation can create:

- wrong assumptions
- bad file targeting
- missing edge cases
- inconsistent architecture

### Better pattern

1. inspect codebase
2. propose plan
3. review plan
4. implement
5. verify

### Memory hook

```text
In code work, planning is damage prevention.
```

---

## 9) Why explicit plans help

A good plan helps both the agent and the human.

### It helps the agent by:

- forcing decomposition
- reducing impulsive edits
- clarifying the target files

### It helps the human by:

- making intent reviewable
- surfacing misunderstandings early
- making approval easier
- improving trust

### Example mini-plan

```text
1. Inspect auth middleware and session validator
2. Update redirect behavior on expired sessions
3. Add unit test for expired session path
4. Run test suite for auth package
```

That already feels much safer than “just code it.”

---

## 10) Core idea #4: code generation is only one phase

A big beginner mistake is treating coding agents as if their entire purpose is:

> generate code

But good coding-agent systems usually include multiple phases:

- read
- understand
- plan
- modify
- verify
- review
- possibly revise

### Why this matters

The code-writing part is only one slice of the actual software task.

### Memory hook

```text
Coding is not just generation. It is generation inside a software loop.
```

---

## 11) Figure: coding loop vs raw code generation

```text
Weak model:
task → generate code → done

Better model:
task → inspect → plan → edit → test → review → done
```

This is one of the chapter’s most important mental models.

---

## 12) Core idea #5: tests are not optional decoration

For coding agents, verification is everything.

### Why?

Because code that looks plausible can still be:

- broken
- untyped
- untested
- logically wrong
- incompatible with the rest of the project

### Strong verification signals include

- unit tests
- integration tests
- lint checks
- type checks
- build success
- targeted runtime checks

### Memory hook

```text
Pretty code is not proof. Passing checks are proof-shaped.
```

---

## 13) Why tests matter even more with agents

Humans often write code from partial understanding.
Agents do too, but faster.

That speed makes testing even more important.

### Without tests

A coding agent can produce:

- very believable wrongness

### With tests

You at least gain:

- a feedback loop
- a way to catch obvious regressions
- a revision trigger

### Builder lesson

A coding agent without verification is a productivity mirage.

---

## 14) Core idea #6: review loops are essential

The chapter naturally fits with the review-loop pattern from earlier multi-agent chapters.

A coding system works better when there is some explicit review stage.

### Review can be:

- human review
- reviewer agent
- automated checks
- or some combination

### Why this matters

A review stage can catch:

- scope drift
- style mismatch
- unsafe changes
- missing tests
- brittle logic
- suspicious shortcuts

### Memory hook

```text
Write, then review. Do not confuse first draft with final patch.
```

---

## 15) Human review vs agent review

### Human review is best for:

- architecture judgment
- subtle correctness
- tradeoffs
- security concerns
- product alignment

### Agent review is useful for:

- style issues
- obvious anti-patterns
- checklist-based critique
- quick iteration

### Best pattern

Combine:

- agent generation
- automated verification
- human review for important changes

This is the production-friendly version of coding-agent usage.

---

## 16) Core idea #7: coding agents should be tool-assisted

A serious coding agent usually needs more than a raw chat model.

Useful tools include:

- file readers
- search tools
- diff tools
- test runners
- linters
- typecheckers
- formatter access
- repo navigation
- issue/task references

### Why this matters

A coding agent that cannot inspect, search, and verify is much weaker.

### Memory hook

```text
A coding agent without tools is mostly autocomplete with ambition.
```

---

## 17) Why repo search is so important

One of the highest-value capabilities for coding agents is search:

- where is this symbol defined?
- where is this pattern already used?
- which tests cover this module?
- which config controls this behavior?

### Why?

Because software changes are rarely isolated from the surrounding codebase.

### Builder lesson

The best coding agents are usually excellent navigators before they are excellent writers.

---

## 18) Core idea #8: smaller edits beat giant rewrites

Coding agents are often strongest on:

- bounded changes
- well-described bugs
- pattern-conforming edits
- clear migration tasks
- local refactors

They are often riskier on:

- giant rewrites
- open-ended architecture redesign
- vague product changes
- broad framework migrations without guardrails

### Memory hook

```text
Coding agents are usually better surgeons than city planners.
```

That is one of the best practical heuristics for using them.

---

## 19) Why broad refactors are dangerous

A broad refactor invites:

- inconsistency
- missed dependencies
- partial migration
- hidden regressions
- confusing diffs

### Better pattern

Break a large refactor into:

1. plan
2. targeted sub-changes
3. checks after each change
4. review gates

### Builder lesson

If the change is huge, turn it into a workflow, not one giant prompt.

---

## 20) Core idea #9: observability matters for coding agents too

Coding agents benefit from the same observability ideas as workflows and other agents.

### You want visibility into:

- what files were read
- what files were changed
- what plan was proposed
- what tests were run
- what failed
- what retry/revision happened

### Why?

Because code changes are high-consequence.
You need to understand the path that led to the patch.

### Memory hook

```text
If the coding agent touched the repo, you should be able to reconstruct why.
```

---

## 21) Core idea #10: evaluation matters for coding agents

A coding agent should be evaluated on more than “did it produce code?”

Possible dimensions:

- task completion
- correctness
- test pass rate
- scope adherence
- edit efficiency
- review burden
- regression rate

### Why this matters

A coding agent that writes many lines of code quickly is not necessarily valuable if:

- it creates cleanup work
- breaks tests
- increases review burden
- drifts from scope

### Builder lesson

The best coding agent is not the one that writes the most.
It is the one that delivers the most reliable accepted changes.

---

## 22) Real-life examples that make Chapter 24 stick

### Example 1: bug fix

Task:

- “Fix the null pointer in checkout summary when shipping address is missing.”

Good coding-agent workflow:

1. inspect erroring module
2. locate affected code path
3. propose small patch
4. add regression test
5. run targeted tests
6. produce diff for review

This is an excellent coding-agent task.

---

### Example 2: small feature addition

Task:

- “Add a retry button when report generation fails.”

Good workflow:

1. inspect existing error UI pattern
2. identify component and handler
3. implement UI + action
4. add or update tests
5. run frontend test/lint checks

Again: scoped, verifiable, high-signal.

---

### Example 3: dangerous open-ended task

Task:

- “Modernize the whole backend architecture.”

This is risky as a single coding-agent prompt.
It should be broken into:

- planning
- milestones
- review checkpoints
- narrow implementation passes

That contrast is one of the chapter’s key lessons.

---

## 23) Tiny code examples that make the chapter memorable

### Example A: scoped task structure

```python
task = {
    "goal": "Fix expired-session redirect",
    "constraints": ["Do not modify unrelated auth flows"],
    "required_checks": ["unit tests", "lint"]
}
```

This captures why structured scoping helps.

---

### Example B: planning before editing

```python
plan = [
    "Inspect auth middleware",
    "Find expired-session path",
    "Update redirect target",
    "Add regression test",
    "Run auth tests"
]
```

This captures the plan-first pattern.

---

### Example C: verification loop

```python
patch = coding_agent(task)
test_result = run_tests("auth")

if not test_result["passed"]:
    patch = coding_agent({"goal": "repair failing tests"})
```

This captures why generation must live inside a test loop.

---

### Example D: reviewer step

```python
review = reviewer_agent({
    "diff": patch,
    "criteria": ["scope adherence", "test coverage", "style consistency"]
})
```

This captures the review-loop pattern.

---

## 24) Common beginner mistakes this chapter prevents

### Mistake 1

“Coding agents should just be asked to code immediately.”

### Better

Inspect and plan first.

---

### Mistake 2

“If the code looks right, it probably is.”

### Better

Run checks and tests.

---

### Mistake 3

“Bigger prompts and bigger edits mean bigger productivity.”

### Better

Smaller, bounded tasks are usually safer and more reliable.

---

### Mistake 4

“The coding agent can review itself well enough.”

### Better

Use explicit review loops and, for important changes, human review.

---

### Mistake 5

“Coding-agent quality equals number of lines produced.”

### Better

Quality is about accepted, correct, scoped changes with low review pain.

---

## 25) The chapter as a decision tree

```text
Need help from a coding agent?
   │
   ├─ Is the task clearly scoped?
   │      └─ No → narrow the task first
   │
   ├─ Does the agent have repo context and tools?
   │      └─ No → provide codebase access and search/check tools
   │
   ├─ Is there a plan?
   │      └─ No → plan before editing
   │
   ├─ Can the change be verified?
   │      └─ Yes → run tests/lint/type checks
   │
   ├─ Is review needed?
   │      └─ Yes → add review loop
   │
   └─ Is the task too large?
          └─ Break it into workflow stages
```

This is the most operational summary of Chapter 24.

---

## 26) Table: all major concepts in Chapter 24

| Concept                      | Meaning                                                | Why it matters                   |
| ---------------------------- | ------------------------------------------------------ | -------------------------------- |
| scoped task                  | clearly bounded code change                            | reduces drift and risk           |
| repo context                 | relevant files, configs, tests, docs                   | needed for grounded edits        |
| plan-first workflow          | inspect and decompose before editing                   | prevents damage                  |
| code generation as one phase | writing code is part of a larger loop                  | more realistic engineering model |
| verification                 | tests, lint, typecheck, build checks                   | catches believable wrongness     |
| review loop                  | human/agent/automated critique after generation        | improves reliability             |
| tool-assisted coding         | search, read, diff, run checks                         | makes coding agent much stronger |
| smaller edits                | bounded changes over giant rewrites                    | safer use pattern                |
| observability                | visibility into agent actions and edits                | helps debugging and trust        |
| coding-agent evaluation      | score by correctness and acceptance, not output volume | better success metric            |

---

## 27) What Chapter 24 is _not_ trying to do

This chapter is not deeply teaching:

- benchmark methodology for coding agents
- language-specific agent tuning
- repo permission models in detail
- CI/CD integration architecture in depth
- multi-agent coding pipelines in full detail

It is doing something more foundational:

> teaching the operating principles that make coding agents useful instead of reckless

That is its real mission.

---

## 28) Best concise explanation of Chapter 24

If you had to explain the whole chapter in 30 seconds:

> Chapter 24 explains that coding agents work best when they are used inside a disciplined software process. Their tasks should be clearly scoped, they should inspect the codebase and plan before editing, and their output should be verified with tests and other checks. Review loops, tool access, observability, and evaluation all matter. The core lesson is that coding agents should act like constrained, tool-assisted collaborators, not free-roaming autonomous programmers.

---

## 29) Final review sheet

### Must-remember facts

- Coding-agent tasks should be tightly scoped.
- Repo context is essential for good code changes.
- Planning should usually happen before editing.
- Code generation is only one phase in a larger software loop.
- Tests and checks are critical.
- Review loops improve reliability.
- Tool access makes coding agents much stronger.
- Smaller edits are safer than giant rewrites.
- Observability and evaluation matter here too.

### Must-remember mental model

```text
Good coding agents operate inside a process, not outside one.
```

### Must-remember builder lesson

Do not ask a coding agent to be a miracle worker.

Ask it to be a disciplined collaborator:

- with scope
- with tools
- with checks
- with review

That is the deep practical lesson of Chapter 24.
