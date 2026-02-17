# Agent memory: persistence, retrieval, and not making a junk drawer (Chapter 7)

## Summary

- Memory is how agents keep continuity beyond a single context window.
- The hard part is **selectivity**:
  - store stable, useful facts
  - avoid sensitive or ephemeral details
  - retrieve only when relevant
- Memory works best when split into:
  - profile (explicit fields)
  - notes (human-readable)
  - vector memory (semantic retrieval)

## Core concepts

### Memory layers

- **Short-term context**: current conversation window.
- **Working memory/state**: current run (workflow variables, intermediate results).
- **Long-term memory**: persistent knowledge (preferences, projects, user facts).

### What to store

Store:

- preferences (tone, format)
- stable facts (timezone, role, recurring constraints)
- long-lived project context

Avoid:

- secrets/credentials
- sensitive personal data
- transient one-offs

### Retrieval strategies

- rules/intent-based (“travel planning” → preferences)
- similarity-based (embeddings)
- hybrid (rules + similarity + recency)

## Code example: simple vector memory (conceptual)

```python
def remember(memory_store, text, metadata):
    emb = embed(text)
    memory_store.upsert(vector=emb, text=text, metadata=metadata)

def recall(memory_store, query, top_k=5, filters=None):
    q_emb = embed(query)
    return memory_store.search(vector=q_emb, top_k=top_k, filters=filters)
```

## Practical takeaways

- Memory quality is a product feature: curate it.
- Add dedup/expiry and user controls (view/edit/delete).
- Don’t retrieve memory every turn. Gate by intent or explicit need.
