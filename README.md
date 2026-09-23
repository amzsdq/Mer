# Mer

Research workspace for finding the optimal boundary between a ChatGPT Automation reservation prompt and GitHub durable control/state.

## Research question
What information must be embedded in the scheduler/automation prompt for reliable cold-start and self-repair, and what information should live in GitHub for low-overhead durable execution?

## Working hypothesis
The leading candidate is a **thin immutable bootstrap prompt + GitHub canonical kernel/spec/status/evidence** architecture.

The reservation prompt should contain only what must exist before GitHub can be trusted/read:
- identity and writable repository
- canonical prompt/kernel path
- same-automation self-update safety invariants
- minimum status/report contract
- source-of-truth rule and failure behavior
- model/reasoning policy

GitHub should contain:
- Goal / Plan / current task / next action
- project context and instructions
- dynamic scheduler/runtime policy
- desired state vs observed state
- evidence and experiment ledger
- canonical reservation prompt used for propagation/repair
- version/hash metadata for drift detection

## Method
Compare three candidates:
1. PROMPT_HEAVY
2. HYBRID_BOOTSTRAP
3. POINTER_ONLY

Measure bootstrap overhead, time-to-first-useful-work, stale-policy incidents, continuation reliability, recovery, repository I/O, prompt mutation frequency, and useful-work utilization.

## Sources
Prior internal evidence is read from `amzsdq/RRULEF`, `amzsdq/tEST`, and `amzsdq/workwork` as READ_ONLY unless a later experiment explicitly changes that scope.
