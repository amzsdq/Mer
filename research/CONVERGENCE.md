# Mer convergence decision

## Decision
Select HYBRID: a small stable execution kernel in the injected prompt plus GitHub-owned dynamic project/program state.

## Evidence table

| Criterion | PROMPT_HEAVY | HYBRID | POINTER_ONLY |
|---|---|---|---|
| Clean-path bootstrap reads | Best: observed 0 before result | Observed 4 in baseline | Observed 4 in clean baseline |
| Dynamic mutation | Requires deployed prompt mutation when embedded generation/marker changes | GitHub-only mutation demonstrated | GitHub-only churn demonstrated; 0 prompt mutations |
| Dynamic split-brain risk | Highest because dynamic values are duplicated/deployed | Low when dynamic values remain GitHub-only | Low for dynamic values |
| Missing GitHub entrypoint | Can retain embedded work state, but risks staleness | Stable kernel can carry canonical recovery references/fail-safe | Fail-safe but cannot reconstruct project state from loss of sole pointer |
| Stale deployed prompt | Embedded dynamic values can stale | Stable kernel changes rarely; dynamic values reload from GitHub | Dynamic freshness good; pointer/schema drift remains single-entrypoint weakness |
| Scheduler accepted/live vs later wake | Same physical limitation without independent observer | Same physical limitation | Same physical limitation |

## Boundary
Prompt owns only stable execution concerns:
- role/identity and repository/write scope;
- authority/delegation contract: GitHub owns dynamic project/program state;
- canonical bootstrap/recovery references sufficient to avoid a single fragile pointer;
- scheduler survival: same automation, recurring RRULE, exact schedule, enabled, no stale DTSTART;
- fail-safe behavior when required state cannot be validated;
- work-session discipline and minimum report contract.

GitHub owns:
- Goal and Master Plan;
- current stage and next step;
- changing experiment/project state;
- workload definitions, evidence, gates, backlog, and final deliverables.

## Remove / reject
- Reject PROMPT_HEAVY embedding of generation, marker, current stage, next action, or other frequently changing project state.
- Reject pure POINTER_ONLY as final architecture because loss/drift of its sole entrypoint leaves no project-state recovery route.
- Remove duplicated dynamic values from the final injected prompt.
- Do not claim WAKE_OK from scheduler write acceptance alone; retain ACCEPTED / LIVE_STATE_VERIFIED / LATER_WAKE as separate evidence states.
- Do not add rules/files unless they protect a demonstrated failure mode or are required by final validation.

## Stage 5 gate
PASS. The smallest currently supported design is HYBRID stable kernel + GitHub dynamic brain. Proceed to Stage 6: three clean end-to-end wakes plus one recovery wake, then finalize prompt/schema/recovery/rollout/rejected-alternatives artifacts.
