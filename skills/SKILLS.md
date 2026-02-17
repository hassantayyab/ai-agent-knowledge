# Skills file (how to use this library effectively)

This file is designed for coding assistants (Cursor, Claude Code, Copilot Chat, etc.) to quickly decide **what to do**, **which chapter(s) to consult**, and **how to produce reliable code** without loading the entire repo.

## 0) Route first (never load everything)

1. Read `INDEX.json`
2. Select **1–3 chapters** that match the task (use `keywords` + `use_when`)
3. Open only those `principles/chapters/*.md` files
4. Produce an answer grounded in those chapters

If information is missing, expand by **+1–2 adjacent chapters**, not more.

---

## 1) Core “skills” and when to use them

### A. Agent design (single-agent)

**Use when:** you’re building a first agent or converting a prompt into an agent loop  
**Do:**

- define success + stop conditions
- choose tools for hard facts
- keep the loop short and observable

**Chapters to consult:**

- Ch 04 (Agents 101)
- Ch 06 (Tool calling)
- Ch 09 (Middleware)

---

### B. Workflows and reliability

**Use when:** you need determinism, retries, branching, and resumability  
**Do:**

- decompose into steps
- add checkpoints for resume
- keep each step small and testable

**Chapters to consult:**

- Ch 12–15 (Workflows, decomposition, checkpoints, streaming)

---

### C. RAG (Retrieval Augmented Generation)

**Use when:** knowledge lives in documents and hallucination risk is high  
**Do:**

- build chunking + indexing pipeline
- enforce ACL filters outside the model
- ground answers with citations
- evaluate retrieval separately from generation

**Chapters to consult:**

- Ch 17–19 (RAG pipeline + retrieval)
- Ch 26 (Evaluating RAG)
- Ch 30 (Prompt injection defense)

---

### D. Observability and debugging

**Use when:** runs are inconsistent, expensive, or failing silently  
**Do:**

- trace every step/tool call
- track cost/latency
- capture inputs/outputs for replay

**Chapters to consult:**

- Ch 16 (Observability / tracing)

---

### E. Evals and quality gates

**Use when:** you’re changing prompts/tools/retrieval and want regressions caught early  
**Do:**

- build a small eval set from real traces
- prefer structured scoring (schemas, tool correctness)
- run regression gates on every change

**Chapters to consult:**

- Ch 25 (Evals 101)
- Ch 26 (RAG evals)
- Ch 27 (Tool calling evals)

---

### F. Prompting + structured outputs

**Use when:** outputs must be machine-consumable or tool args must be correct  
**Do:**

- define explicit output contracts
- use JSON schema / enums for routing
- validate → repair → fail-safe

**Chapters to consult:**

- Ch 28 (Prompt engineering)
- Ch 29 (Structured outputs)

---

### G. Security (prompt injection, tool boundaries)

**Use when:** agent browses, retrieves, or touches sensitive systems  
**Do:**

- treat retrieved text as untrusted evidence
- enforce least privilege + allowlists in middleware
- validate/redact outputs
- test adversarial cases

**Chapters to consult:**

- Ch 09 (Middleware)
- Ch 30 (Prompt injection defense)

---

### H. Memory (continuity across sessions)

**Use when:** personalization or long-running projects need continuity  
**Do:**

- store only stable high-signal facts
- avoid sensitive data
- separate workflow state vs long-term memory
- add retrieval gates + curation

**Chapters to consult:**

- Ch 31 (Memory 101)
- Ch 32 (Memory in workflows + multi-agent)

---

### I. Multi-agent systems

**Use when:** specialization/parallelism is clearly needed (not as default)  
**Do:**

- start with supervisor + workers
- use schema-first handoffs + evidence fields
- orchestrate via workflow graphs for determinism

**Chapters to consult:**

- Ch 21–24 (Multi-agent patterns + orchestration)
- Ch 23 (Handoffs)

---

### J. Code generation (ship-safe)

**Use when:** you want the model to write/modify code reliably  
**Do:**

- diff-first changes (patches over rewrites)
- run verification loop (lint/typecheck/tests/build)
- repair based on real errors
- restrict write scope and require review for risky actions

**Chapters to consult:**

- Ch 33 (Code generation)
- Ch 29 (Structured outputs)
- Ch 25 (Evals)

---

## 2) Default output format for coding help

When asked to implement something, respond with:

1. **Chapters used:** (numbers + titles)
2. **Plan:** 3–6 steps
3. **Implementation:** minimal diffs or file-level changes
4. **Verification:** exact commands/checks to run
5. **Risk notes:** security + failure modes + rollback

---

## 3) Minimal “router” prompt snippet

Use this pattern in tools that support a persistent instruction:

> Always route using `INDEX.json`. Select 1–3 chapters by `keywords/use_when`. Open only those files. Then answer with: Chapters used → Plan → Implementation → Verification → Risks.
