# Mer Goal

Find and empirically validate the relay operating policy that maximizes long-run useful-work utilization while preserving continuation and recoverability.

## Priority order
1. Continuity / recoverability: do not lose the relay or authoritative state.
2. Useful-work duty cycle: maximize genuine goal-directed work and minimize idle/bootstrap/control overhead.
3. Simplicity: among candidates with equivalent measured reliability and utilization, prefer the simpler mechanism.

## Method
The goal is not to invent a plausible architecture. The goal is to converge experimentally:
hypothesis -> controlled test -> evidence -> keep/revise/reject -> next discriminating hypothesis.

Evidence from tEST, workwork, or external systems is prior evidence only until reproduced or otherwise justified for Mer.

## Completion condition
Research is not COMPLETE merely because a prompt/repository architecture works.
Completion requires an empirically selected operating policy with:
- repeated end-to-end continuation success;
- measured useful-work/idle behavior;
- tested scheduler timing and planned-gap policy;
- measured close/handoff reserve;
- prompt/canonical synchronization behavior validated;
- adverse recovery tests;
- comparison against at least one plausible simpler or competing candidate;
- no unresolved candidate likely to materially improve the primary objective without a declared reason not to test it.

Any claim of reliability is bounded to the tested conditions; do not claim absolute platform guarantees.
