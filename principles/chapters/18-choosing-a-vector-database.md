# Choosing a vector database: the boring parts that matter (Chapter 18)

## Summary

- A vector database stores **embeddings + metadata** and supports fast **similarity search** for RAG.
- Many options are “good enough”; selection is mostly about ops and product constraints rather than raw recall.
- Key decision factors:
  - scale (documents, QPS)
  - latency requirements
  - filtering/metadata support
  - hybrid search (keyword + vector)
  - hosting (managed vs self-hosted)
  - ecosystem + SDK ergonomics
- Often, a general database with vector support can be enough early on.

## Core concepts

### What a vector database does

For each chunk, you store:

- vector embedding
- text chunk
- metadata (doc ID, section, timestamp, permissions, tags)

Then you query with:

- a query embedding
- optional filters (e.g., customer_id, permissions)
  to return top-K similar chunks.

### When “any vector DB” is fine

Early prototypes usually care more about:

- developer speed
- simple APIs
- cost predictability

You can switch later if you keep your retrieval layer abstracted.

### What usually matters most

- **Metadata filtering**: critical for multi-tenant / permissioned RAG.
- **Hybrid search**: combining keyword + vector often improves practical retrieval.
- **Operational ease**: backups, monitoring, upgrades, scaling.
- **Latency**: retrieval must be fast enough to not dominate response time.

### Common approaches

- Dedicated vector DBs (managed or self-hosted)
- Traditional DBs with vector extensions (often enough for small/medium scale)

## Practical takeaways

- Pick a DB that makes **filtering + ops** easy; pure vector similarity is rarely the bottleneck.
- Abstract retrieval behind an interface so you can swap vendors.
- Decide based on your constraints: multi-tenancy, latency, and team ops comfort.
