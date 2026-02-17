# Prompt injection defense: securing tools, RAG, and agent boundaries (Chapter 30)

## Summary

- Prompt injection is the core adversary for tool-using agents.
- Rule: treat all external text (web/docs/email) as **untrusted evidence**, not instructions.
- Defend in layers:
  - tool allowlists + least privilege
  - ACL-filtered retrieval
  - output validation + redaction
  - sandbox/HITL for high-risk actions
  - adversarial evals

## Core concepts

### Where to enforce security

- Put permissions and ACL enforcement in middleware (outside the model).
- Don’t rely on prompts alone.

### Adversarial evals

Include:

- injected instructions inside retrieved docs
- malicious tool instructions
- “leak secrets” attempts

## Practical takeaways

- Assume retrieved text is hostile until proven otherwise.
- Enforce tool permissions and retrieval filters at system boundaries.
- Test jailbreaks continuously.
