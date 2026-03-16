# Chapter 23 — Building Multi-Agent Systems

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 23** clearly and memorably using diagrams, tables, examples, and practical engineering framing.

---

## 1) Summary of the chapter

Chapter 23 explains how to actually **build** multi-agent systems in practice after understanding the basic idea of specialization and supervision.

The chapter teaches these core ideas:

1. Multi-agent systems are built by combining:
   - specialized agents
   - coordination logic
   - tool interfaces
   - workflows
2. The supervisor pattern is one of the easiest ways to structure them.
3. Each agent should have:
   - a clear role
   - clear boundaries
   - a clean interface
4. Multi-agent design is not mainly about adding lots of agents.
5. It is mainly about:
   - task decomposition
   - role assignment
   - routing
   - review
   - orchestration
6. A good multi-agent system should feel like a well-run team, not a chaotic group chat.
7. The chapter’s deeper message is that most multi-agent systems should be simple, intentional, and role-driven.

That is the whole chapter in one line:

```text
A good multi-agent system is a carefully organized team of specialized agents with clear roles, interfaces, and coordination paths.
```

---

## 2) The chapter in one figure

```text
User task
   ↓
Coordinator / supervisor
   ├─ Agent A: specialist role
   ├─ Agent B: specialist role
   ├─ Agent C: specialist role
   ↓
Results combined / reviewed / routed
   ↓
Final output
```

This is the chapter’s center of gravity.

---

## 3) Why this chapter matters

After learning:

- what agents are
- how tools work
- how workflows work
- how supervisors work

the next practical question is:

> “How do I put all this together into a real multi-agent system?”

That is what Chapter 23 is about.

### Why this is important

A lot of people hear “multi-agent” and imagine:

- very advanced systems
- agent swarms
- complicated messaging graphs
- exotic frameworks

This chapter pushes the design back toward first principles:

- roles
- responsibilities
- coordination
- clean interfaces

### Memory hook

```text
Multi-agent systems are designed teams, not magic swarms.
```

---

## 4) Core idea #1: start from task decomposition

The most important first step in building a multi-agent system is usually:

> break the overall job into distinct kinds of work

### Why this matters

If you cannot explain the different sub-jobs, you probably do not yet need multiple agents.

### Good decomposition questions

- What separate roles exist in this process?
- Which steps need different tools or context?
- Which steps benefit from different prompts or memories?
- Where does review or validation belong?
- Which work is planning, and which work is execution?

### Example

For a writing workflow:

- planner
- drafter
- editor
- publisher

For a coding workflow:

- planner
- implementer
- reviewer
- tester

### Memory hook

```text
Multi-agent starts with task decomposition, not with counting agents.
```

---

## 5) Why not every system needs many agents

A major hidden lesson in this chapter is restraint.

### Bad instinct

> “This task is complex, so let’s add five agents.”

### Better instinct

> “What distinct roles actually justify separate agents?”

### Why this matters

Too many agents can create:

- unnecessary coordination cost
- harder debugging
- more latency
- role confusion
- more brittle behavior

### Builder lesson

The goal is not “more agents.”
The goal is “better separation of responsibilities.”

---

## 6) Core idea #2: agents need clear roles

Once you decompose the task, each agent should have a clear role.

### A clear role usually means

- narrow responsibility
- specific expertise
- defined input/output expectations
- known tools or resources
- known handoff rules

### Bad role

```text
Agent 1 does random project stuff
```

### Better role

```text
Planner agent creates implementation plans
Reviewer agent checks correctness and quality
```

### Why role clarity matters

Without clear roles:

- agents duplicate work
- agents conflict
- orchestration becomes vague
- evaluation becomes harder

### Memory hook

```text
Blurry roles create blurry systems.
```

---

## 7) Figure: vague roles vs clear roles

```text
Bad:
  Agent A = does many things
  Agent B = also does many things
  Agent C = maybe helps somehow

Better:
  Planner = plan
  Researcher = gather evidence
  Writer = draft
  Reviewer = critique
```

This is one of the chapter’s most practical design lessons.

---

## 8) Core idea #3: interfaces matter

A multi-agent system is not just a set of roles.
It is also a set of **interfaces**.

### What an interface means here

Each agent should have a clear contract:

- what it receives
- what it returns
- what format it uses
- what job it is expected to do

### Why this matters

If agents hand vague prose to each other, the system gets messy quickly.

### Better pattern

Use structured handoffs when possible.

### Example structured handoff

```json
{
  "task_type": "draft_blog_post",
  "audience": "security founders",
  "outline": ["problem", "approach", "examples"],
  "tone": "clear and practical"
}
```

### Memory hook

```text
Agent interfaces should be as clean as tool interfaces.
```

That is a very strong practical design rule.

---

## 9) Why structured handoffs are so useful

Structured outputs between agents help with:

- consistency
- debugging
- observability
- traceability
- downstream correctness

### Compare

#### Weak handoff

> “Here’s some thoughts. Maybe continue from here.”

#### Better handoff

```json
{
  "summary": "...",
  "open_questions": ["..."],
  "recommended_next_step": "editor_review"
}
```

### Builder lesson

Multi-agent systems become much easier to operate when agents talk through structured artifacts instead of fuzzy prose alone.

---

## 10) Core idea #4: the supervisor pattern is a strong default

Building on Chapter 22, this chapter strongly implies that a **supervisor-led hierarchy** is usually the cleanest place to start.

### Why?

Because one agent can:

- own routing
- own sequencing
- own delegation
- own final assembly

while specialists stay focused on their narrower work.

### Benefits of supervisor-first design

- easier orchestration
- clearer accountability
- easier tracing
- fewer peer-to-peer surprises
- cleaner extensibility later

### Memory hook

```text
If you do not know how to coordinate multiple agents, start with one supervisor.
```

---

## 11) Figure: supervisor-led multi-agent design

```text
Supervisor
   ├─ delegates to specialist A
   ├─ delegates to specialist B
   ├─ checks whether review is needed
   └─ returns final result
```

This is the most stable pattern to remember from the chapter.

---

## 12) Core idea #5: agents can be wrapped as tools

This continues the previous chapter’s most practical trick.

### Meaning

A specialist agent can often be wrapped and exposed like a callable tool to another agent.

### Why this is useful

Because then orchestration logic becomes much simpler:

- the supervisor chooses which specialist-tool to call
- the specialist does the job
- the supervisor decides what happens next

### Why this matters

It means you can reuse much of the mental model from tool-calling systems.

### Memory hook

```text
Multi-agent orchestration often becomes simpler when specialist agents are callable like tools.
```

---

## 13) Core idea #6: workflows and multi-agent systems overlap

This chapter sits very close to the workflow chapters conceptually.

### Why?

Because multi-agent systems often need:

- sequencing
- branching
- review loops
- merge steps
- conditions

That means many multi-agent systems are also workflow systems.

### Plain-English explanation

A multi-agent system is often:

```text
workflow control flow
      +
specialized agent roles
```

### Why this matters

It stops you from treating “workflow” and “multi-agent” as totally separate worlds.
In practice they often blend together.

---

## 14) Figure: workflow + agents

```text
Workflow graph
   ├─ step 1 = planner agent
   ├─ step 2 = researcher agent
   ├─ step 3 = writer agent
   └─ step 4 = reviewer agent
```

### Builder lesson

Sometimes the best multi-agent system is just a workflow whose steps happen to be specialized agents.

---

## 15) Review loops are a major multi-agent pattern

One of the most natural uses for multiple agents is review.

### Example pattern

- creator agent produces draft
- reviewer agent critiques it
- creator revises
- supervisor decides whether quality is good enough

### Why this matters

Review is often easier to isolate as a separate role than trying to make one agent both create and reliably critique itself.

### Memory hook

```text
Creation + review is one of the cleanest justifications for multiple agents.
```

---

## 16) Core idea #7: coordination logic should be explicit

A common mistake is to let coordination be vague:

- maybe agent A decides things
- maybe agent B asks agent C
- maybe everyone improvises

The better pattern is to make coordination explicit.

### Explicit coordination means

- who delegates to whom
- when a handoff happens
- who can call which agent
- who decides completion
- where review happens
- where escalation happens

### Why this matters

Explicit coordination makes the system:

- easier to test
- easier to monitor
- easier to change
- easier to understand

### Memory hook

```text
Hidden coordination creates hidden bugs.
```

---

## 17) What makes a multi-agent system “good”

A good multi-agent system usually has:

- justified specialization
- clear boundaries
- minimal overlap
- clear orchestration
- traceable handoffs
- visible control flow
- measurable quality

### What makes a bad one

- too many agents
- unclear roles
- overlapping responsibilities
- hidden delegation logic
- noisy peer-to-peer chatter
- hard-to-debug results

### Table: good vs bad multi-agent system

| Good                              | Bad                     |
| --------------------------------- | ----------------------- |
| clear roles                       | vague roles             |
| supervisor or clear orchestration | unclear coordination    |
| structured handoffs               | sloppy prose handoffs   |
| few justified agents              | many unnecessary agents |
| traceable flow                    | opaque flow             |

---

## 18) Real-life examples that make Chapter 23 stick

### Example 1: writing pipeline

Agents:

- planner
- writer
- editor
- publisher supervisor

Flow:

1. planner proposes outline
2. writer drafts
3. editor revises
4. publisher decides if it is publishable

This is a near-perfect multi-agent teaching example.

---

### Example 2: coding pipeline

Agents:

- planner
- implementation agent
- code reviewer
- test-analysis agent
- supervisor

Flow:

1. planner produces implementation plan
2. implementer writes patch
3. tests run
4. test-analysis agent interprets failures
5. reviewer critiques code quality
6. supervisor decides next loop or completion

This is a highly realistic modern setup.

---

### Example 3: research assistant system

Agents:

- search agent
- summarizer agent
- verifier agent
- final answer composer
- coordinator

Flow:

1. search gathers candidates
2. summarizer condenses them
3. verifier checks support
4. composer produces final answer
5. coordinator manages the order

---

### Example 4: support operations

Agents:

- triage agent
- account lookup agent
- solution drafting agent
- escalation agent
- supervisor

This is useful when different parts of the support workflow need different prompts, permissions, or tools.

---

## 19) Tiny code examples that make the chapter memorable

### Example A: specialist agent roles

```python
def planner_agent(task):
    return {"plan": f"Plan for {task}"}

def writer_agent(plan):
    return {"draft": f"Draft based on {plan['plan']}"}

def reviewer_agent(draft):
    return {"feedback": f"Feedback on {draft['draft']}"}
```

This captures role separation.

---

### Example B: supervisor orchestration

```python
def supervisor(task):
    plan = planner_agent(task)
    draft = writer_agent(plan)
    feedback = reviewer_agent(draft)
    return {
        "plan": plan,
        "draft": draft,
        "feedback": feedback
    }
```

This captures the orchestration pattern.

---

### Example C: structured handoff

```python
handoff = {
    "task": "write landing page copy",
    "audience": "B2B security buyers",
    "constraints": ["clear", "short", "trustworthy"]
}
```

This captures clean interfaces between agents.

---

### Example D: review loop

```python
draft = writer_agent(plan)
review = reviewer_agent(draft)

if "major issues" in review["feedback"]:
    draft = writer_agent({"plan": plan["plan"] + " with fixes"})
```

This captures the creation-review-revision idea.

---

## 20) Why observability matters even more here

Multi-agent systems can become hard to debug fast.

### Why?

Because now the system has:

- multiple role boundaries
- multiple outputs
- multiple handoffs
- multiple possible failure points

### What you want visibility into

- which agent was called
- with what input
- what it returned
- what the coordinator did next
- how long each sub-step took
- which revision loop happened

### Builder lesson

If single-agent systems need tracing, multi-agent systems need it even more.

---

## 21) Why evaluation matters even more here

The moment you have multiple agents, you can no longer only evaluate the final answer.

You may also want to evaluate:

- each specialist’s output
- handoff quality
- reviewer usefulness
- supervisor decisions
- end-to-end system quality

### Plain-English explanation

A multi-agent system creates more surfaces to evaluate, not fewer.

### Memory hook

```text
More agents = more places quality can improve or break.
```

---

## 22) Common beginner mistakes this chapter prevents

### Mistake 1

“Multi-agent means lots of agents.”

### Better

It means justified specialization with clear coordination.

---

### Mistake 2

“Agents can just talk freely and figure it out.”

### Better

Explicit orchestration is usually much safer.

---

### Mistake 3

“The supervisor should also be the best specialist.”

### Better

Let specialists specialize and let the supervisor manage.

---

### Mistake 4

“Prose handoffs are good enough.”

### Better

Structured handoffs usually make systems much more reliable.

---

### Mistake 5

“Workflow thinking and multi-agent thinking are separate.”

### Better

In practice, multi-agent systems often sit on top of workflow control flow.

---

## 23) The chapter as a decision tree

```text
Need multiple agents?
   │
   ├─ No → single agent may be enough
   │
   └─ Yes
        │
        ├─ Can you clearly decompose the task into roles?
        │      └─ Yes → define specialist agents
        │
        ├─ Need coordination?
        │      └─ Yes → use a supervisor
        │
        ├─ Need reliable handoffs?
        │      └─ Yes → use structured outputs/interfaces
        │
        ├─ Need review?
        │      └─ Add reviewer specialist or revision loop
        │
        └─ Need predictable execution?
               └─ combine multi-agent roles with workflow control flow
```

This is the most operational summary of Chapter 23.

---

## 24) Table: all major concepts in Chapter 23

| Concept               | Meaning                                         | Why it matters                        |
| --------------------- | ----------------------------------------------- | ------------------------------------- |
| task decomposition    | split big job into distinct sub-roles           | starting point for multi-agent design |
| specialist agent      | agent with narrow responsibility                | cleaner quality boundaries            |
| clear role definition | agent has a known job and scope                 | avoids overlap and confusion          |
| interface / handoff   | what one agent passes to another                | critical for reliability              |
| structured handoff    | machine-usable transfer format                  | better orchestration and debugging    |
| supervisor pattern    | manager agent coordinates specialists           | easiest orchestration default         |
| agents as tools       | wrap specialist agents as callable capabilities | simplifies routing                    |
| review loop           | one agent critiques another’s work              | common high-value multi-agent pattern |
| workflow + agents     | control flow plus specialized roles             | realistic system design               |
| explicit coordination | clear routing and decision logic                | prevents chaos                        |

---

## 25) What Chapter 23 is _not_ trying to do

This chapter is not deeply teaching:

- decentralized agent swarms
- negotiation protocols
- multi-agent memory-sharing internals
- distributed scheduling
- advanced concurrency
- market-style agent systems

It is doing something more practical:

> showing how to build multi-agent systems as clear, role-based, supervised software systems

That is its real mission.

---

## 26) Best concise explanation of Chapter 23

If you had to explain the whole chapter in 30 seconds:

> Chapter 23 explains that building a good multi-agent system starts with decomposing a task into clearly justified specialist roles. Each agent should have a clear responsibility and a clean interface, ideally using structured handoffs. The supervisor pattern is a strong default, because one coordinating agent can call specialist agents, often wrapped like tools, and manage the overall flow. The deeper message is that multi-agent systems should look like well-organized teams, not uncontrolled swarms.

---

## 27) Final review sheet

### Must-remember facts

- Multi-agent systems should start from task decomposition.
- Agents need clear roles and boundaries.
- Handoffs between agents should ideally be structured.
- The supervisor pattern is a strong default for coordination.
- Specialist agents can often be wrapped as tools.
- Many multi-agent systems are really workflows plus specialized roles.
- Review loops are one of the cleanest use cases for multiple agents.
- Observability and evaluation become even more important in multi-agent systems.

### Must-remember mental model

```text
Good multi-agent systems are organized teams, not chaotic swarms.
```

### Must-remember builder lesson

Do not begin multi-agent design by asking how many agents you can add.

Begin by asking:

- what roles actually exist?
- how should they coordinate?
- what should each one own?

That is the deep practical lesson of Chapter 23.
