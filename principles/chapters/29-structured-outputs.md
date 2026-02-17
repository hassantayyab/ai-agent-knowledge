# Structured outputs: schemas, validators, and reliable parsing (Chapter 29)

## Summary

- Free-form text is fragile. **Structured outputs** make agent results machine-usable and testable.
- Pattern: schema-first prompting → validate → repair → fail-safe.
- Use enums for routing and keep schemas minimal.

## Core concepts

### Validation and repair loop

1. Constrain output to JSON schema
2. Parse + validate
3. If invalid, retry with validation errors
4. Stop safely after N attempts

### Schema design

- minimal required fields
- enums for decision points
- separate structured `result` from optional `notes`

## Practical takeaways

- If code consumes it, use schemas.
- Validate everything; implement repair retries.
