# Chapter 8 — Dynamic Agents

_Complete study guide in markdown, designed for memory, clarity, and fast review_

> Goal: explain **every concept in Chapter 8** clearly and memorably using diagrams, tables, examples, and code.

---

## 1) Summary of the chapter

Chapter 8 explains **dynamic agents**: agents whose behavior can change at runtime instead of being fixed forever at creation time.

The chapter teaches six core ideas:

1. The simplest agent is **static**:
   - fixed instructions
   - fixed model
   - fixed tools
2. A **dynamic agent** computes those things at runtime.
3. Dynamic agents are useful when behavior should change based on:
   - user tier
   - language
   - permissions
   - environment
4. Dynamic agents are more powerful, but less predictable.
5. Runtime context becomes a major design tool.
6. Once agents become dynamic, you usually also need stronger **middleware** and **policy controls**.

That is the whole chapter in one line:

```text
Static agents are easier to reason about.
Dynamic agents are more adaptable, but need more control.
```

---

## 2) The chapter in one figure

```text
Static agent
┌────────────────────────────┐
│ prompt fixed               │
│ model fixed                │
│ tools fixed                │
└────────────────────────────┘

Dynamic agent
┌────────────────────────────┐
│ runtime context arrives    │
│ ├─ choose instructions     │
│ ├─ choose model            │
│ └─ choose tools            │
└────────────────────────────┘
```

---

## 3) Core idea #1: what “dynamic” means

A static agent is created once and behaves the same way for every request.

A dynamic agent changes behavior **at runtime**.

### What can change dynamically?

- instructions / system prompt
- selected model
- available tools
- memory settings
- behavior style
- support level
- permissions

### Plain-English explanation

A dynamic agent is an agent that says:

> “Before I answer, I will first look at who this user is and what situation I’m in.”

That is the essence of the chapter.

---

## 4) Static vs dynamic: the core contrast

### Static agent

Same behavior every time.

```text
All users
   ↓
same prompt
same model
same tools
```

### Dynamic agent

Behavior changes based on context.

```text
User + request context
   ↓
different prompt / model / tools
```

### Table: static vs dynamic

| Static agent   | Dynamic agent      |
| -------------- | ------------------ |
| simpler        | more adaptable     |
| predictable    | more context-aware |
| easier to test | harder to test     |
| fewer branches | more branches      |
| less flexible  | more personalized  |

---

## 5) Why dynamic agents exist

The chapter explains that dynamic agents are useful when different users or situations need different behavior.

### Examples

- free vs paid vs enterprise users
- English vs non-English users
- simple user vs advanced technical user
- internal employee vs external customer
- safe environment vs restricted environment

### Builder lesson

A single rigid agent often cannot serve every situation well.
Dynamic agents solve that by adapting.

---

## 6) The central tradeoff: predictability vs power

This is the chapter’s most important conceptual point.

### Dynamic agents give more power because they can:

- personalize
- gate features
- choose stronger or cheaper models
- expose different tools to different users
- adapt language and tone automatically

### But they reduce predictability because:

- more branching means more possible behaviors
- testing becomes harder
- debugging becomes harder
- consistency can decrease

### Figure: the tradeoff

```text
More dynamic behavior
        ▲
        │  more power
        │  more personalization
        │  more flexibility
        │
        │  but also
        │  more complexity
        │  less predictability
        ▼
```

### Memory hook

```text
Dynamic = powerful, but slippery
```

---

## 7) Core idea #2: runtime context drives behavior

The chapter’s main technical mechanism is **runtime context**.

Runtime context means information available at request time, such as:

- user tier
- language
- account type
- environment
- permissions
- location
- feature flags

### Why this matters

Instead of hardcoding one universal behavior, the agent can inspect runtime context and adapt.

### Simple mental model

Runtime context is like the information badge handed to the agent before it starts working.

---

## 8) Figure: runtime context as an input

```text
User request
     +
Runtime context
  ├─ user tier
  ├─ language
  ├─ environment
  ├─ permissions
  └─ feature flags
        ↓
Agent config for this run
  ├─ instructions
  ├─ model
  └─ tools
```

This is the architectural heart of the chapter.

---

## 9) Dynamic instructions

One thing a dynamic agent can change is its **instructions**.

### Why this matters

Different users may need:

- different tone
- different support depth
- different explanation level
- different policies

### Example

A beginner user might need:

- simpler explanations
- more hand-holding

An enterprise user might need:

- technical depth
- best practices
- escalation options

So the prompt can be generated dynamically from runtime context.

---

## 10) Dynamic model selection

The chapter also shows that an agent can choose a different model depending on runtime context.

### Why would you do this?

Because not every request deserves the same model.

### Examples

- enterprise users get stronger model
- free users get cheaper model
- complex tasks get stronger model
- simple tasks get faster model

### Memory hook

```text
Dynamic agent = routing built inside the agent
```

This connects back to Chapter 5.

---

## 11) Dynamic tools

The chapter also shows that the toolset itself can change.

### Why?

Because not all users should access the same capabilities.

### Examples

- free tier gets basic tools
- pro tier gets analytics tools
- enterprise tier gets custom integration tools

This is very important in real products, because tools represent capability and cost.

### Builder lesson

Dynamic agents are not only about changing wording.
They are also about changing what the system can do.

---

## 12) Code example from the chapter

This is the chapter’s key code example:

```ts
const supportAgent = new Agent({
  name: 'Dynamic Support Agent',

  instructions: async ({ runtimeContext }) => {
    const userTier = runtimeContext.get('user-tier');
    const language = runtimeContext.get('language');

    return `You are a customer support agent for our SaaS platform.
The current user is on the ${userTier} tier and prefers ${language} language.

For ${userTier} tier users:
${userTier === 'free' ? '- Provide basic support and documentation links' : ''}
${userTier === 'pro' ? '- Offer detailed technical support and best practices' : ''}
${userTier === 'enterprise' ? '- Provide priority support with custom solutions' : ''}

Always respond in ${language} language.`;
  },

  model: ({ runtimeContext }) => {
    const userTier = runtimeContext.get('user-tier');
    return userTier === 'enterprise' ? openai('gpt-4') : openai('gpt-3.5-turbo');
  },

  tools: ({ runtimeContext }) => {
    const userTier = runtimeContext.get('user-tier');
    const baseTools = [knowledgeBase, ticketSystem];

    if (userTier === 'pro' || userTier === 'enterprise') {
      baseTools.push(advancedAnalytics);
    }

    if (userTier === 'enterprise') {
      baseTools.push(customIntegration);
    }

    return baseTools;
  },
});
```

---

## 13) What this code teaches

This is one of the best code examples in the early part of the book because it shows dynamic behavior in three separate dimensions:

### A) Dynamic instructions

The prompt changes by:

- user tier
- language

### B) Dynamic model

The selected model changes by:

- user tier

### C) Dynamic tools

The available tools change by:

- user tier

### That is the full pattern

```text
runtime context
   ↓
instructions / model / tools
```

That is the whole chapter in code form.

---

## 14) Breaking the code example down

### `runtimeContext.get("user-tier")`

This fetches user segmentation info.

Possible values:

- free
- pro
- enterprise

### `runtimeContext.get("language")`

This fetches language preference.

### Why this matters

The agent is not guessing who the user is.
It is given structured context from the application.

This is an important design lesson:

- the app provides context
- the agent consumes it

---

## 15) Dynamic instructions in the example

The instruction block generates different support behavior.

### For free users

- basic support
- documentation links

### For pro users

- deeper technical support
- best practices

### For enterprise users

- priority support
- custom solutions

### Why this is such a good example

Because it is easy to understand in business terms.
You can immediately see why one-size-fits-all behavior is not ideal.

---

## 16) Dynamic language behavior

The example also changes output language:

```ts
Always respond in ${language} language.
```

### Why this matters

Dynamic behavior can also reflect:

- localization
- personalization
- accessibility
- region-specific needs

This shows that dynamic agents are not only about pricing tiers.
They are also about user preferences.

---

## 17) Dynamic model routing in the example

The code routes enterprise users to a stronger model:

```ts
return userTier === 'enterprise' ? openai('gpt-4') : openai('gpt-3.5-turbo');
```

### What this teaches

A dynamic agent can bake model routing into runtime policy.

### Why a business might do this

- premium users get premium quality
- stronger model cost is justified for high-value customers
- cheaper models protect margins on low-tier users

### Builder lesson

This is where technical design meets pricing strategy.

---

## 18) Dynamic tool gating in the example

The code also adjusts available tools:

### Base tools

All users get:

- knowledge base
- ticket system

### Extra tools

Pro and enterprise users get:

- advanced analytics

Enterprise users also get:

- custom integration

### Why this matters

This is a very realistic product pattern:

- not all customers should get the same capabilities
- some tools cost more
- some tools require more permissions
- some tools represent premium product value

---

## 19) Figure: dynamic support agent by tier

```text
Free user
  ├─ basic support prompt
  ├─ cheaper model
  └─ base tools only

Pro user
  ├─ deeper support prompt
  ├─ better support behavior
  └─ base tools + analytics

Enterprise user
  ├─ priority/custom prompt
  ├─ stronger model
  └─ base tools + analytics + custom integrations
```

This figure captures the whole chapter very well.

---

## 20) Why dynamic agents are useful in enterprise systems

Enterprise systems often need policy differences based on:

- customer tier
- department
- permissions
- contract level
- region
- environment
- feature rollout stage

A static agent cannot handle all of that elegantly.

Dynamic agents are a natural fit because they allow one logical system to behave differently depending on business rules.

### Real-life examples

- enterprise admin gets more tool access than normal employee
- finance queries use stricter tools than marketing queries
- beta users get experimental features
- regulated region gets stricter output policy

---

## 21) Dynamic agents connect product logic to agent behavior

This chapter is really about connecting:

- business logic
- application state
- user segmentation
- product rules

to:

- agent instructions
- tool access
- model selection

### In plain English

It is where the agent starts behaving like a real product component, not just a chatbot.

---

## 22) Risks of dynamic agents

The chapter is not just celebrating dynamic behavior. It is warning you, indirectly, that dynamic behavior has costs.

### Main risks

- too many branches
- confusing behavior differences
- hard testing matrix
- hidden policy complexity
- harder debugging
- inconsistent user experience

### Table: dynamic benefits vs costs

| Benefit            | Cost                   |
| ------------------ | ---------------------- |
| personalization    | more branching         |
| premium tiering    | more policy logic      |
| model optimization | harder testing         |
| tool gating        | more access complexity |
| localization       | more runtime state     |

---

## 23) Why middleware becomes more important after this chapter

Once agent behavior depends on runtime context, you need strong controls around:

- permissions
- policies
- safety rules
- who gets which tools
- who gets which model access

That is why the next conceptual step is middleware.

### Memory hook

```text
Dynamic agents make middleware more important, not less.
```

Because the more runtime variation exists, the more carefully you need to control it.

---

## 24) Dynamic agents vs cloned agents

A beginner might think:

> instead of one dynamic agent, why not just make many separate agents?

That can work sometimes.

### But dynamic agents can be better because:

- less duplication
- one central code path
- easier shared maintenance
- easier common upgrades

### But many separate agents may be better when:

- behaviors are truly very different
- policies are radically different
- teams own them independently

### Builder lesson

Dynamic agents help when the behaviors are related variations of one core role.

---

## 25) Real-life examples that make Chapter 8 stick

### Example 1: support assistant by tier

Exactly like the chapter:

- free → docs + basic support
- pro → richer help
- enterprise → stronger model + premium capabilities

---

### Example 2: multilingual assistant

Runtime context includes:

- language = Arabic
- user role = admin

The agent then:

- replies in Arabic
- exposes admin tools
- uses instructions appropriate for admin workflows

---

### Example 3: environment-aware coding agent

Runtime context includes:

- environment = production
- environment = local dev

In production:

- stricter instructions
- safer tools
- read-only mode

In dev:

- more flexible tooling

This is a perfect dynamic-agent pattern.

---

## 26) Tiny code examples that make the chapter memorable

### Example A: dynamic tone

```python
def get_instructions(user_tier):
    if user_tier == "enterprise":
        return "Provide high-touch, detailed support."
    return "Provide concise standard support."
```

This is the simplest version of dynamic instructions.

---

### Example B: dynamic model choice

```python
def choose_model(user_tier):
    if user_tier == "enterprise":
        return "strong_model"
    return "cheap_model"
```

This is the simplest version of runtime model routing.

---

### Example C: dynamic tools

```python
def get_tools(user_tier):
    tools = ["knowledge_base", "ticket_system"]
    if user_tier in ["pro", "enterprise"]:
        tools.append("analytics")
    if user_tier == "enterprise":
        tools.append("custom_integration")
    return tools
```

This is the simplest version of dynamic tool gating.

---

## 27) Common beginner mistakes this chapter prevents

### Mistake 1

“One agent must behave exactly the same for all users.”

### Better

Sometimes the right design is context-aware behavior.

---

### Mistake 2

“Dynamic just means changing the prompt.”

### Better

Dynamic can also mean changing:

- model
- tools
- behavior rules
- memory settings

---

### Mistake 3

“More dynamic behavior is always better.”

### Better

More dynamic behavior gives more power, but also less predictability.

---

### Mistake 4

“I can just hide all runtime branching in the prompt.”

### Better

Runtime context should be explicit and controlled by the application.

---

### Mistake 5

“Dynamic behavior removes the need for strong system design.”

### Better

Dynamic behavior increases the need for:

- middleware
- permissions
- testing
- observability

---

## 28) The chapter as a decision tree

```text
Should agent behavior vary by user/context?
   │
   ├─ No → static agent may be enough
   │
   ├─ Yes → dynamic agent
   │
   ├─ Need different instructions?
   │      └─ generate prompt from runtime context
   │
   ├─ Need different model quality/cost?
   │      └─ route model dynamically
   │
   ├─ Need different capabilities?
   │      └─ gate tools dynamically
   │
   └─ More runtime variation introduced?
          └─ strengthen middleware + testing
```

---

## 29) Table: all major concepts in Chapter 8

| Concept                 | Meaning                                | Why it matters                 |
| ----------------------- | -------------------------------------- | ------------------------------ |
| static agent            | fixed instructions/model/tools         | predictable baseline           |
| dynamic agent           | runtime-configured agent behavior      | adaptability                   |
| runtime context         | request-time app/user/environment data | drives adaptation              |
| dynamic instructions    | prompt changes by context              | personalization                |
| dynamic model           | model chosen by context                | quality/cost optimization      |
| dynamic tools           | toolset chosen by context              | feature gating and permissions |
| predictability vs power | main tradeoff in the chapter           | core design decision           |
| tier-based support      | chapter’s main business example        | makes the concept concrete     |

---

## 30) What Chapter 8 is _not_ trying to do

This chapter is not yet deeply teaching:

- middleware enforcement details
- feature flag infrastructure
- policy engines
- evaluation for dynamic branches
- advanced access control systems

It is introducing the concept:

> an agent does not have to be fixed forever

That is the key idea.

---

## 31) Best concise explanation of Chapter 8

If you had to explain the whole chapter in 30 seconds:

> A dynamic agent is an agent whose instructions, model, and tools can change at runtime based on context like user tier, language, or permissions. This makes agents more adaptable and useful for real products, especially enterprise systems, but it also makes them harder to test and reason about. The central tradeoff is predictability versus power.

---

## 32) Final review sheet

### Must-remember facts

- Static agents are fixed.
- Dynamic agents adapt at runtime.
- Runtime context drives that adaptation.
- Instructions, model, and tools can all be dynamic.
- Dynamic behavior is powerful but less predictable.
- More dynamics means stronger need for middleware and testing.

### Must-remember mental model

```text
Dynamic agent = one agent shell, different runtime configuration
```

### Must-remember builder lesson

Before making an agent dynamic, ask:

> “What should vary, and why?”

If the answer is:

- user tier
- permissions
- language
- environment
- premium capabilities

then dynamic design may be worth it.

But if the answer is vague, static may be better.

That is the deep practical lesson of Chapter 8.
