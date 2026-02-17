# Building a RAG pipeline: ingestion, chunking, retrieval, and guardrails (Chapter 19)

## Summary

- A production RAG pipeline is more than “embed + search”.
- You need clear stages:
  1. ingestion/parsing
  2. chunking + metadata
  3. embedding + indexing
  4. retrieval (vector + filters + optional hybrid search)
  5. generation with grounding and citations
- Quality is driven by:
  - chunking strategy
  - query formulation
  - filtering/permissions
  - reranking (optional)
  - evaluation with real queries
- You should add guardrails:
  - avoid leaking unauthorized docs
  - avoid answering beyond retrieved evidence

## Core pipeline stages

### 1) Ingestion & parsing

Goal: convert PDFs/HTML/docs into clean text with structure.

- preserve headings, section boundaries, timestamps, authors when possible
- normalize formatting artifacts

### 2) Chunking + metadata

Chunking makes documents retrievable. Metadata makes results usable and safe.

**Recommended metadata**

- document ID + title
- section/heading path
- timestamp/version
- tenant/customer/user permission keys
- source URL or file path

### 3) Embedding & indexing

- compute embeddings for each chunk
- store in vector DB with metadata
- keep an update strategy (re-index on change)

### 4) Retrieval

A practical retrieval query uses:

- vector similarity
- filters (tenant/permissions)
- sometimes keyword search (hybrid)

**Good defaults**

- retrieve `topK` chunks (start small, e.g., 3–8)
- include neighbor chunks if needed (like `messageRange` in memory)

### 5) Generation (grounded)

Prompt the model to:

- answer using retrieved chunks
- cite sources (chunk metadata)
- refuse/ask for clarification if evidence is missing

## Reliability upgrades (optional but common)

### Reranking

Vector similarity is not perfect. A reranker can reorder candidates for better relevance.

### Query rewriting

Rewrite the user query into a better retrieval query:

- expand abbreviations
- include synonyms
- add context (product name, feature name)

### Answer verification

Before returning:

- check that key claims appear in retrieved chunks
- otherwise downgrade confidence or ask follow-ups

## Practical takeaways

- Treat RAG as a pipeline with multiple quality levers, not a single database call.
- Metadata and filtering are as important as embeddings for real products.
- Add “don’t answer beyond evidence” as a first-class instruction in your generation prompt.
