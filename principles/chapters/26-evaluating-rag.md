# Evaluating RAG: retrieval quality, grounding, and answer faithfulness (Chapter 26)

## Summary

- RAG evals must test:
  1. **Retrieval quality** (did you fetch the right chunks?)
  2. **Generation faithfulness** (did you use chunks correctly?)
- Many RAG failures are retrieval failures in disguise.
- Good suites include adversarial cases (ambiguous queries, near-duplicate docs).

## Core concepts

### Retrieval metrics

- **Recall@k**: did a relevant chunk appear in top-k?
- **Precision@k**: how many of top-k are truly relevant?

Requires “gold” doc/chunk labels per query.

### Grounding / faithfulness checks

- citations present
- key claims supported by retrieved text
- refuses/asks clarification when evidence missing

### Debugging with traces

Inspect:

- which chunks were retrieved
- filters used (tenant/ACL)
- which chunks were actually cited/used

## Practical takeaways

- Score retrieval and generation separately so you know what to fix.
- Build gold sets from real user questions and the docs they should use.
