# Inter-agent communication: handoffs, schemas, and trust boundaries (Chapter 23)

## Summary

- Multi-agent systems live or die by **handoffs**: outputs become future inputs.
- Use **structured handoffs** (templates/JSON schemas) to prevent ambiguity.
- Define **trust boundaries**:
  - treat worker outputs as untrusted until verified
  - require evidence fields for claims
- Keep handoffs small to avoid context bloat.

## Core concepts

### Why handoffs amplify errors

A hallucination can propagate:

- worker invents → supervisor accepts → another worker builds on it → final answer bakes it in

### Recommended handoff fields (pattern)

- `task`
- `assumptions`
- `results`
- `evidence` (citations, IDs, links)
- `unknowns`
- `next_steps`

### Trust boundaries + verification

- verify key claims via tools/RAG/cross-check workers
- add a verifier worker for high-stakes cases
- explicitly mark uncertainty instead of guessing

### Context budgeting

- don’t forward full transcripts
- forward structured results + minimal evidence references

## Practical takeaways

- Schema-first handoffs by default.
- Evidence-required outputs reduce compounding hallucinations.
- Keep messages small; merge deterministically.
