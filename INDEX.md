# INDEX

Use this index to pick the **minimum** relevant chapters. Default: open **≤ 2** chapters.

## Principles of Building AI Agents

### Ch 01: From “Attention” to ChatGPT: the modern LLM timeline

**Use when:** Use when you need context on where LLMs came from and why 2017/2022 mattered.

**Best for:** background, product context

**Keywords:** history, transformers, chatgpt, tokens

→ See: `./principles/chapters/01-from-attention-to-chatgpt.md`

### Ch 02: Choosing models like an engineer: provider, size, context, reasoning

**Use when:** Use when deciding hosted vs OSS, model size tradeoffs, context windows, or reasoning models.

**Best for:** model selection, architecture tradeoffs

**Keywords:** provider, latency, cost, context window, reasoning

→ See: `./principles/chapters/02-choosing-provider-and-model.md`

### Ch 03: Prompt engineering that actually works: examples, system prompts, and structure

**Use when:** Use when prompts are inconsistent and you need better control via examples, formatting, and constraints.

**Best for:** prompt design, reliability

**Keywords:** prompting, few-shot, system prompt, formatting

→ See: `./principles/chapters/03-writing-great-prompts.md`

### Ch 04: Agents 101

**Use when:** Use when you’re deciding whether you need an agent and want the core mental model (agent loop, autonomy, tools, state).
**Best for:** Defining an agent vs workflow; core components; a minimal agent loop implementation.
**Keywords:** agents, agent loop, autonomy, tools, state, workflow
→ See: `./principles/chapters/04-agents-101.md`

### Ch 05: Model Routing and Structured Output

**Use when:** Use when you need to route requests to different models and require structured outputs for automation or branching.
**Best for:** Tiered routing (cheap→strong), fallbacks, and schema-first JSON outputs with validation/repair.
**Keywords:** model routing, structured output, schemas, validation, fallback, cost, latency
→ See: `./principles/chapters/05-model-routing-and-structured-output.md`

### Ch 06: Tool Calling

**Use when:** Use when your agent must call tools/APIs reliably and safely.
**Best for:** Tool specs, argument validation, retries, budgeting tool calls, and safe interpretation of results.
**Keywords:** tool calling, function calling, validation, retries, budgets, tool specs
→ See: `./principles/chapters/06-tool-calling.md`

### Ch 07: Agent Memory

**Use when:** Use when you want persistence/personalization across sessions without turning memory into a junk drawer.
**Best for:** Memory layers, what to store/avoid, retrieval strategies, and basic vector memory patterns.
**Keywords:** memory, long-term memory, vector memory, retrieval, personalization, hygiene
→ See: `./principles/chapters/07-agent-memory.md`

### Ch 08: Dynamic Agents

**Use when:** Use when you want agents that adapt prompts/tools/plans at runtime (intent modes, escalation, gating).
**Best for:** Mode switching, toolset gating, structured routing decisions, and observability for dynamic behavior.
**Keywords:** dynamic agents, routing, modes, escalation, tool gating, observability
→ See: `./principles/chapters/08-dynamic-agents.md`

### Ch 09: Agent Middleware

**Use when:** Use when you need a reliability/safety layer outside the model (ACLs, retries, validation, tracing).
**Best for:** Middleware pipeline patterns, tool governance, permissions enforcement, and post-processing.
**Keywords:** middleware, permissions, ACL, validation, redaction, tracing, budgets
→ See: `./principles/chapters/09-agent-middleware.md`

### Ch 10: Popular Third-Party Tools

**Use when:** Use when you’re selecting third-party tooling for orchestration, RAG, evals, tracing, or model gateways.
**Best for:** Ecosystem map and decision criteria; avoiding lock-in via interfaces; tooling-by-category.
**Keywords:** frameworks, tools, orchestration, RAG, evals, tracing, model gateway
→ See: `./principles/chapters/10-popular-third-party-tools.md`

### Ch 11: Model Context Protocol: a USB‑C for agent tools

**Use when:** you need a standard way to connect agents/models to tools across providers and want tool portability.
**Best for:** MCP server/client mental model; wiring tools via MCP; composing tool gateways via a proxy server.
**Keywords:** MCP, model context protocol, servers, clients, tools, portability, registries, stdio, SSE
→ See: `./principles/chapters/11-mcp-model-context-protocol.md`

### Ch 12: Workflows 101: when agents have too much freedom

**Use when:** you need predictable, structured execution (graphs/decision trees) instead of fully autonomous agent behavior.
**Best for:** decomposing tasks into steps; branching/parallelism/checkpoints; adding tracing to multi-step logic.
**Keywords:** workflows, graphs, decision trees, branching, parallel execution, checkpoints, tracing
→ See: `./principles/chapters/12-workflows-101.md`

### Ch 13: Workflow graph patterns: branching, chaining, merging, and conditions

**Use when:** you’re designing workflow graphs and need reliable control flow primitives (branching/merging/conditions).
**Best for:** decomposing into small nodes; binary decision points; fan-out/fan-in merges with clear schemas.
**Keywords:** workflows, branching, chaining, merging, conditions, fan-out, fan-in, control flow
→ See: `./principles/chapters/13-workflow-graph-patterns.md`

### Ch 14: Suspend and resume: workflows that wait for humans and external events

**Use when:** your workflow must pause for approvals, webhooks, or long-running external jobs.
**Best for:** durable state + checkpointing; idempotent side effects; correlating external events to workflow instances.
**Keywords:** suspend, resume, checkpoints, durable state, approvals, webhooks, idempotency, correlation IDs
→ See: `./principles/chapters/14-suspend-and-resume.md`

### Ch 15: Streaming updates: making long agent workflows feel alive

**Use when:** your agent/workflow takes seconds+ and you need progress visibility and better UX.
**Best for:** token streaming vs event streaming; designing progress events and state transitions for UIs.
**Keywords:** streaming, token streaming, event streaming, progress updates, lifecycle events, UX
→ See: `./principles/chapters/15-streaming-updates.md`

### Ch 16: Observability and tracing: debugging agents like distributed systems

**Use when:** you need to debug/iterate on agent behavior and want evidence (what happened) instead of guesswork.
**Best for:** tracing model/tool/retrieval steps; structured logs + metrics; building evals from real failures.
**Keywords:** observability, tracing, spans, logs, metrics, debugging, retries, failures
→ See: `./principles/chapters/16-observability-and-tracing.md`

### Ch 17: RAG 101: retrieval beats stuffing context

**Use when:** your agent needs external/private/up-to-date knowledge that shouldn’t be shoved into the prompt.
**Best for:** understanding the RAG pipeline; chunking + metadata; grounding answers with retrieved context.
**Keywords:** RAG, retrieval-augmented generation, embeddings, vector database, chunking, grounding, citations
→ See: `./principles/chapters/17-rag-101.md`

### Ch 18: Choosing a vector database: the boring parts that matter

**Use when:** you’re picking storage for embeddings and need to decide between vector DBs vs DBs-with-vectors.
**Best for:** filtering/permissions, hybrid search, ops constraints, and retrieval abstraction decisions.
**Keywords:** vector database, embeddings, similarity search, metadata filtering, hybrid search, multi-tenant, RAG
→ See: `./principles/chapters/18-choosing-a-vector-database.md`

### Ch 19: Building a RAG pipeline: ingestion, chunking, retrieval, and guardrails

**Use when:** you’re implementing RAG end-to-end and need practical stages + quality levers.
**Best for:** ingestion/chunking/metadata, retrieval filters, grounded generation prompts, and reliability upgrades (rerank, rewrite).
**Keywords:** RAG pipeline, ingestion, parsing, chunking, metadata, retrieval, filtering, reranking, query rewriting, grounding
→ See: `./principles/chapters/19-setting-up-your-rag-pipeline.md`

### Ch 20: Alternatives to RAG: when retrieval isn’t the best hammer

**Use when:** you’re considering non-RAG approaches (fine-tune, cache, structured DB/tools, long context) or hybrid designs.
**Best for:** choosing the simplest mechanism that meets correctness + latency + cost + governance.
**Keywords:** alternatives to RAG, fine-tuning, caching, precompute, structured databases, knowledge graphs, long context window
→ See: `./principles/chapters/20-alternatives-to-rag.md`

### Ch 21: Multi-agent systems 101: specialization, coordination, and failure modes

**Use when:** a single agent/workflow isn’t enough and you need specialization or parallelism.
**Best for:** supervisor/worker architecture; multi-agent failure modes; stabilization tactics (schemas, budgets, termination).
**Keywords:** multi-agent, supervisor, workers, delegation, coordination, parallelism, failure modes, termination
→ See: `./principles/chapters/21-multi-agent-101.md`

### Ch 22: The supervisor pattern: one boss, many specialists

**Use when:** Use when you want to start multi-agent systems with a controlled delegation pattern (one coordinator, multiple specialists).
**Best for:** Designing supervisor + worker roles, delegation budgets, stop conditions, and merge/reconcile behavior.
**Keywords:** supervisor, multi-agent, delegation, workers, coordination, merge, budgets, stop conditions
→ See: `./principles/chapters/22-supervisor-pattern.md`

### Ch 23: Inter-agent communication: handoffs, schemas, and trust boundaries

**Use when:** Use when agents need to hand off work to other agents and you want reliable, low-bloat communication.
**Best for:** Handoff schemas, trust boundaries, evidence/unknowns fields, and verification to prevent error propagation.
**Keywords:** handoffs, inter-agent communication, schemas, trust boundaries, verification, context budgeting, evidence
→ See: `./principles/chapters/23-inter-agent-communication.md`

### Ch 24: Multi-agent workflows: combining specialization with graph control

**Use when:** Use when you want multi-agent specialization but need workflow-level determinism (branches, merges, checkpoints).
**Best for:** Workflow-as-orchestrator patterns, fan-out/fan-in, conditional escalation, and standardizing merge schemas and budgets.
**Keywords:** multi-agent workflows, orchestration, graphs, fan-out, fan-in, escalation, merges, checkpoints
→ See: `./principles/chapters/24-multi-agent-workflows.md`

### Ch 25: Evals 101: measuring agent quality without vibes

**Use when:** Use when you need to measure and improve agent reliability (correctness, tool use, policy, consistency).
**Best for:** Eval types (unit/e2e/regression), scoring methods (schema checks, heuristics, LLM-judge), and building datasets from traces.
**Keywords:** evals, evaluation, regression, LLM judge, rubric, datasets, scoring, quality
→ See: `./principles/chapters/25-evals-101.md`

### Ch 26: Evaluating RAG: retrieval quality, grounding, and answer faithfulness

**Use when:** Use when you need to measure RAG performance and distinguish retrieval failures from generation faithfulness failures.
**Best for:** Recall@k/precision@k, grounding checks, gold sets, and adversarial RAG eval cases.
**Keywords:** RAG evals, recall@k, precision@k, grounding, faithfulness, citations, gold set, adversarial queries
→ See: `./principles/chapters/26-evaluating-rag.md`

### Ch 27: Evaluating tool calling: selection, args, robustness, interpretation

**Use when:** Use when you need to measure tool-use reliability (selection, arguments, retries, interpretation).
**Best for:** Building tool-call datasets from traces; schema-based scoring; mocked tools for deterministic eval runs.
**Keywords:** tool calling evals, tool selection, arguments, schema validation, retries, robustness, traces, mocking
→ See: `./principles/chapters/27-evaluating-tool-calling.md`

### Ch 28: Prompt engineering for agents: instructions that actually stick

**Use when:** Use when you’re writing system prompts for agents and need clarity on roles, constraints, tool rules, and output contracts.
**Best for:** Prompt structure, common failure fixes, and treating prompts like versioned code tied to evals/traces.
**Keywords:** prompt engineering, system prompt, instructions, constraints, tool rules, output format, examples, versioning
→ See: `./principles/chapters/28-prompt-engineering.md`

### Ch 29: Structured outputs: schemas, validators, and reliable parsing

**Use when:** Use when the agent output must be machine-consumable (automation, workflow routing, storage, scoring).
**Best for:** Schema-first prompting, validation/repair loops, enum-based routing fields, and production-safe parsing.
**Keywords:** structured output, schema, JSON, validation, repair loop, enums, parsing, determinism
→ See: `./principles/chapters/29-structured-outputs.md`

### Ch 30: Prompt injection defense: securing tools, RAG, and agent boundaries

**Use when:** Use when you’re building agents that browse/retrieve/call tools and need a layered security posture.
**Best for:** Treating retrieved text as untrusted; tool allowlists/least privilege; ACL-filtered retrieval; output validation/redaction; adversarial evals.
**Keywords:** prompt injection, jailbreaks, untrusted content, least privilege, allowlist, ACL, RAG security, redaction
→ See: `./principles/chapters/30-prompt-injection-defense.md`

### Ch 31: Memory 101: what to remember, where to store it, and when to retrieve

**Use when:** Use when you want agents to persist information across turns/sessions without creating clutter or privacy risk.
**Best for:** Memory layers (short-term/working/long-term), storage types (KV/notes/vector), retrieval strategies, memory hygiene.
**Keywords:** memory, personalization, long-term memory, working memory, vector memory, retrieval, hygiene, dedup, expiry
→ See: `./principles/chapters/31-memory-101.md`

### Ch 32: Memory in workflows and multi-agent systems: shared state without chaos

**Use when:** Use when you need shared state across workflow nodes or agents without letting memory become noisy or unsafe.
**Best for:** Separating workflow state vs long-term memory; memory curation/curator roles; retrieval gates; preventing speculative memories.
**Keywords:** workflow state, shared memory, curation, memory curator, retrieval gates, checkpoints, handoffs, schemas
→ See: `./principles/chapters/32-memory-in-workflows-and-multi-agent.md`

### Ch 33: Code generation: making agents write code you can ship

**Use when:** Use when you want an agent (or coding assistant) to generate/modify code safely and reliably, not as a one-shot code dump.
**Best for:** Diff-first codegen, scaffolding/context control, verification loops (lint/typecheck/tests/build), and repair based on real errors.
**Keywords:** code generation, diffs, patches, verification loop, tests, lint, typecheck, repair, CI
→ See: `./principles/chapters/33-code-generation.md`

### Ch 34: What’s next: building your agent roadmap

**Use when:** Use when you want a pragmatic, ROI-driven sequence for building agents from v1 to workflows, evals, RAG, memory, and multi-agent.
**Best for:** Roadmapping and prioritization; avoiding premature complexity; choosing the simplest architecture that meets requirements and is testable.
**Keywords:** roadmap, progression, v1 agent, workflows, evals, observability, RAG, memory, multi-agent
→ See: `./principles/chapters/34-whats-next-roadmap.md`
