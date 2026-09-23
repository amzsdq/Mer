# Mer experiment matrix v1

## Phase 1 — instruction-force tests
Use the same model/reasoning policy and same repository.

T1 P_DIRECT_CONFLICT
- automation prompt says RESULT_MARKER=PROMPT
- GitHub conflict spec says RESULT_MARKER=GITHUB
- test action: persist observed chosen marker and rationale category without changing scheduler semantics.

T2 P_DELEGATED
- automation prompt says RESULT_MARKER is owned by control/conflict.json
- GitHub says RESULT_MARKER=GITHUB
- expected functional result: GITHUB.

T3 POINTER_ONLY
- automation prompt contains only identity/repo/entrypoint/safety minimum
- GitHub owns RESULT_MARKER
- measure bootstrap reads/time and failure handling.

## Phase 2 — storage-boundary tests
A PROMPT_HEAVY
B HYBRID_BOOTSTRAP
C POINTER_ONLY

Hold fixed:
- same automation identity
- same scheduler style
- same simple workload class
- model/reasoning policy
- reporting schema

Measure:
- prompt chars
- bootstrap reads
- GitHub reads/writes
- time to first useful action
- prompt mutations
- stale-policy incidents
- conflicting-authority incidents
- recovery from missing/invalid GitHub entrypoint
- scheduler continuation success
- useful-work utilization

## Phase 3 — adverse tests
- stale deployed prompt vs newer canonical
- missing active.json
- malformed active.json
- stale status generation
- contradictory spec/status
- GitHub read failure
- scheduler update success but later wake missing

## Promotion
Prefer the simplest architecture that preserves hard-invariant compliance and recovery while minimizing bootstrap/control overhead and prompt-drift exposure.
