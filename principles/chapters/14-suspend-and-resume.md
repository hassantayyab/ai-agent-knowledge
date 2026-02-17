# Suspend and resume: workflows that wait for humans and external events (Chapter 14)

## Summary

- Many real workflows can’t finish in one run because they must **wait**:
  - for a human approval
  - for an external system callback
  - for time (cooldowns, schedules)
- A reliable workflow engine supports **suspend/resume** so long-running processes don’t become “state spaghetti”.
- The core requirement is **durable state**: store where you are in the graph and what data you have so far.

## Core concepts

### Why suspend/resume exists

Agents and workflows often interact with the real world:

- approvals (legal, finance, manager)
- external jobs (data pipelines, payments, provisioning)
- async integrations (webhooks)

If you can’t suspend/resume cleanly, you end up with:

- brittle cron hacks
- manual “restart from step 0”
- half-finished tasks with lost context

### What “suspend” means

Suspending means:

- checkpointing the workflow’s state
- persisting intermediate outputs
- stopping execution safely

### What “resume” means

Resuming means:

- reloading state + intermediate outputs
- continuing from the checkpoint node
- optionally re-validating inputs (auth, freshness)

### Key design points

- **Idempotency:** resuming shouldn’t duplicate side-effects
- **Correlation IDs:** tie external events back to the right workflow instance
- **Timeouts & retries:** suspended workflows need expiry and reattempt policies

## Practical takeaways

- If a workflow touches humans or external systems, plan for suspend/resume early.
- Treat checkpoints as first-class artifacts: store state and intermediate results explicitly.
- Make side-effecting steps idempotent so retries/resumes don’t cause double actions.
