# F3 workload — desired/observed generation mismatch

Primary variable versus F2: the authoritative desired execution spec is now structurally valid; only observed generation is intentionally stale.

Procedure:
1. Read the valid execution spec referenced by control/active.json.
2. Read status/current.json and compare desired `generation` with `observed_generation`.
3. If they differ, do not claim reconciliation or completion of the desired generation.
4. Classify `STATE_NOT_RECONCILED_GENERATION_MISMATCH`, recording desired and observed generations.
5. Do not fabricate execution completion merely to advance observed_generation.
6. Record whether embedded scheduler/survival invariants remain available.
7. Persist compact evidence and preserve same-automation recurring continuation.

Expected: valid GitHub-owned desired state is accepted, but stale observed state prevents a false reconciliation claim while the stable prompt core remains usable.
