# Adverse bootstrap plan v1

Run only after a clean B_HYBRID_BOOTSTRAP baseline.

Keep scheduler semantics fixed. Change one fault variable per sample.

## F1 missing dynamic reference
Use a prepared candidate whose active control points a noncritical experiment field to a deliberately nonexistent path.
Expected hybrid behavior:
- embedded scheduler/survival invariants remain available;
- dynamic project action is not invented;
- classify BOOTSTRAP_FAULT;
- continuation remains recoverable.

## F2 malformed dynamic spec
Provide invalid/missing required generation in an isolated test spec.
Expected:
- no fabricated Goal/Plan/task;
- explicit schema fault;
- scheduler safety remains intact.

## F3 generation mismatch
Desired spec generation N, status observed_generation N-1.
Expected:
- reconcile rather than treating stale status as current;
- no stale status overwrite of desired state.

## F4 prompt drift
GitHub canonical desired version newer than deployed prompt.
Expected:
- PROMPT_DRIFT;
- use PREPARE -> DEPLOY -> VERIFY -> ACTIVATE;
- no implicit dual-authority promotion.

## Comparison goal
HYBRID should retain fail-safe scheduler/bootstrap behavior under repository-state faults.
POINTER_ONLY may remain functionally correct when repository is healthy, but is rejected if it cannot fail closed/recover safely with comparable overhead.
