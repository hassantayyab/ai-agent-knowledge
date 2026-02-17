# AI Agent Knowledge Base

An LLM-optimized knowledgebase for building production-grade AI agents. This repository contains comprehensive principles and patterns extracted from practical experience building reliable, scalable agent systems.

## What's Inside

This repository is structured as a **retrieval-first knowledge base** designed to help you quickly find exactly what you need without loading everything at once.

### 📚 34 Principle Chapters

Comprehensive coverage from foundations to advanced topics:

- **Foundations** (Ch 01-03): LLM timeline, model selection, prompt engineering
- **Agent Architecture** (Ch 04-11): Agent loops, routing, tools, memory, dynamic agents, middleware, MCP
- **Workflows** (Ch 12-15): Structured execution, graph patterns, suspend/resume, streaming
- **Observability** (Ch 16): Tracing, debugging, and monitoring
- **RAG Systems** (Ch 17-20): Retrieval pipelines, vector databases, alternatives
- **Multi-Agent** (Ch 21-24): Supervisor patterns, communication, orchestration
- **Quality & Testing** (Ch 25-27): Evals, RAG testing, tool call evaluation
- **Production** (Ch 28-33): Prompting, structured outputs, security, memory, code generation
- **Roadmap** (Ch 34): Building your agent roadmap

### 🗂️ Quick Navigation

- **`INDEX.md`** — Browse all chapters with keywords and use-cases
- **`INDEX.json`** — Machine-readable chapter index for programmatic access
- **`SKILLS.md`** — Task-based guide: "I need to build X, which chapters do I read?"
- **`ROUTER.md`** — Quick routing guide for common tasks
- **`CLAUDE.md`** — Project instructions for Claude Code

## How to Use This Repository

### The Retrieval-First Workflow

**Don't read everything.** This library is designed for targeted retrieval:

1. **Start with INDEX.md** — Scan chapter titles, keywords, and "use when" descriptions
2. **Pick 1-2 chapters** that match your current task
3. **Read only those chapters** and apply their patterns
4. **Expand if needed** — Add 1 more chapter if information is missing

### Quick Start Examples

**Building your first agent?**

```
→ Read: Ch 04 (Agents 101), Ch 06 (Tool Calling)
```

**Need RAG for document search?**

```
→ Read: Ch 17 (RAG 101), Ch 19 (RAG Pipeline), Ch 26 (Evaluating RAG)
```

**Agent behaving unreliably?**

```
→ Read: Ch 16 (Observability), Ch 25 (Evals 101), Ch 09 (Middleware)
```

**Switching from agent to workflow?**

```
→ Read: Ch 12 (Workflows 101), Ch 13 (Graph Patterns)
```

**Need multi-agent coordination?**

```
→ Read: Ch 21 (Multi-Agent 101), Ch 22 (Supervisor Pattern)
```

## Repository Structure

```
ai-agent-library/
├── README.md              # This file
├── INDEX.md               # Human-readable chapter index
├── INDEX.json             # Machine-readable chapter index
├── SKILLS.md              # Task-based routing guide
├── ROUTER.md              # Quick reference for common tasks
├── CLAUDE.md              # Instructions for Claude Code
├── principles/
│   ├── chapters/          # 34 principle chapters (01-34)
│   └── appendix/          # Additional resources (multimodal, etc.)
└── .cursor/
    └── rules/             # Cursor IDE rules for retrieval-first usage
```

## For AI Coding Assistants

If you're an AI assistant (Cursor, Claude Code, Copilot, etc.) working with this repository:

1. **Always read INDEX.md first** to understand available chapters
2. **Select minimum relevant chapters** (default: ≤2 chapters)
3. **Cite your sources** using format: `Principles Ch XX, section "Title"`
4. **Never load all chapters** — use targeted retrieval only
5. See `CLAUDE.md` and `.cursor/rules/` for detailed instructions

## Core Principles

This library emphasizes:

- ✅ **Start simple** — Single agent before multi-agent, workflows before complex orchestration
- ✅ **Observability first** — You can't improve what you can't measure
- ✅ **Structured over free-form** — Schemas, validation, and contracts prevent chaos
- ✅ **Security by design** — Treat all external content as untrusted, enforce least privilege
- ✅ **Test everything** — Build evals from real traces, catch regressions early
- ✅ **Incremental complexity** — Add features only when needed, not speculatively

## Citation Style

When referencing content from this library:

```
**Ref:** Principles Ch 06, section "Tool Argument Validation"
**Ref:** Principles Ch 19, section "Chunking Strategies"
```

Always include chapter number and section heading for traceability.

## Who This Is For

- **Engineers** building production AI agent systems
- **Architects** designing agent workflows and multi-agent orchestration
- **AI researchers** implementing reliable agent behaviors
- **Product teams** evaluating agent capabilities and limitations
- **AI coding assistants** providing grounded, reference-backed guidance

## Getting Started

1. **Browse INDEX.md** to see all available chapters
2. **Identify your current challenge** (unreliable outputs? RAG? multi-agent?)
3. **Read 1-2 relevant chapters** from `principles/chapters/`
4. **Apply patterns and code examples** directly to your project
5. **Iterate** — Return for more chapters as your system evolves

## Contributing & Feedback

This is a living knowledge base. If you find gaps, outdated patterns, or want to contribute:

- Open an issue for discussion
- Submit PRs with improvements (follow existing chapter structure)
- Share real-world agent patterns that work

## License

[Add license information here]

---

**Remember:** The goal isn't to read everything—it's to find exactly what you need, when you need it. Start with INDEX.md and let your specific challenges guide you through the chapters.
