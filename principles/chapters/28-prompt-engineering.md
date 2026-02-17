# Prompt engineering for agents: instructions that actually stick (Chapter 28)

## Summary

- Agent prompts are operational specs: role, objectives, constraints, tool rules, and output contracts.
- Strong prompts include:
  - explicit “don’ts”
  - schema-based outputs
  - a few targeted examples for edge cases
- Version prompts like code and gate changes with evals.

## Core concepts

### Prompt components

1. Role
2. Objectives (success criteria)
3. Constraints (policies, privacy, safety)
4. Tool rules (when/how to call; what to do on errors)
5. Output contract (schema/sections)
6. Examples

### Common fixes

- ignores tools → strengthen tool rules + add arg schema
- inconsistent format → explicit contract + examples
- hallucinations → require grounding/citations + refusal when evidence missing

## Practical takeaways

- Use prompts to define **policy + interface**, not just tone.
- Tie prompt changes to traces + regression evals.
