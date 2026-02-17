# Alternatives to RAG: when retrieval isn’t the best hammer (Chapter 20)

## Summary

- RAG is common, but not always optimal.
- Alternatives (often combined with RAG):
  - **Fine-tuning** for stable style/format and small knowledge deltas
  - **Cached / precomputed answers** for common queries
  - **Structured databases / knowledge graphs** for precise queries
  - **Long context windows** (brute-force) for small corpora or early prototypes
- The best choice depends on:
  - how often data changes
  - required precision
  - cost/latency
  - governance/permissions

## Core options

### 1) Fine-tuning

**Best for:**

- consistent output format
- domain tone/style
- smaller, stable knowledge patterns

**Not best for:**

- frequently changing facts
- large corpora that constantly update

### 2) Caching and precomputation

If a small set of questions dominates, precompute:

- answers
- embeddings
- retrieval results
  and serve quickly.

**Best for:** cost reduction and latency.

### 3) Structured sources (DBs, APIs, knowledge graphs)

If you need correctness for things like:

- “What is the current plan price?”
- “Which customers are on tier X?”
  then querying structured sources beats fuzzy retrieval.

**Pattern:** tool calling + structured output, not pure RAG.

### 4) Long context windows (brute force)

Sometimes you can just include:

- the whole doc
- a small codebase
- a single policy manual
  and avoid retrieval complexity.

**Works when:** corpora is small and stable, and costs are acceptable.

## Practical decision guide

Choose based on your problem:

- dynamic facts → structured DB/tools
- repetitive FAQs → caching/precompute
- consistent output needs → fine-tune
- small corpus → long context window
- large/permissioned corpus → RAG (with filtering)

## Practical takeaways

- Don’t default to RAG: pick the simplest mechanism that meets correctness + cost.
- Many production systems combine approaches: tools + caching + RAG + (sometimes) fine-tuning.
