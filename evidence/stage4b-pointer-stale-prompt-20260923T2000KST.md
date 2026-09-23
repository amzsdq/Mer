# Stage 4B — C_POINTER_ONLY stale deployed prompt discrimination

Time: 2026-09-23 20:00 KST
Candidate: C_POINTER_ONLY / 0.5.0-C-POINTER

## Condition
Synthetic/reversible stale-prompt condition. Keep the deployed pointer candidate structurally unchanged while treating repository dynamic state as newer authoritative state. Canonical repository state is not destroyed.

## Observed boundary
The deployed C prompt contains stable identity, repository, scheduler survival/fail-safe rules, and a pointer to `control/active.json`; it does not embed generation, desired marker, current stage, or next action. Current authoritative `spec/execution.json` is generation 2 with desired_marker `GITHUB_DYNAMIC_MUTATED`.

## Outcome
PASS for dynamic-state freshness: because the changing values are not duplicated in the deployed C prompt, stale prompt age alone does not make those values stale. On wake, C follows `control/active.json` and reconstructs current repository state. A stale *pointer/schema contract* remains a separate failure class already exposed by Stage 4A; if the entrypoint itself moves or becomes unreadable, C cannot self-recover from project state.

## Discrimination
- Dynamic value churn: C avoids prompt/repository split-brain because dynamic values live only in GitHub.
- Stable pointer drift: C remains vulnerable because its only bootstrap entrypoint is embedded in the prompt.
- Therefore Stage 4B supports separating a small stable execution kernel from dynamic project state rather than embedding dynamic values in the prompt.

Gate: 1/1 PASS.
