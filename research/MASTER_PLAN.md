# Mer Master Plan v1

## Rule
Each wake follows the current stage and next step. It does not choose a new experiment ad hoc. New ideas go to backlog unless they invalidate the current test.

## Stage 0 — Reconnaissance — COMPLETE
Compare RRULEF, tEST, workwork and external controller/durable-workflow patterns.

## Stage 1 — Instruction authority — COMPLETE
Test direct prompt instruction, explicit delegation to GitHub, and pointer-only behavior.

## Stage 2 — Hybrid failure handling — COMPLETE
Test missing reference, malformed state, generation mismatch, semantic conflict, and prompt drift.

## Stage 3 — Normal-path boundary cost — IN PROGRESS
3A HYBRID baseline — COMPLETE: 2 clean samples, 4 bootstrap reads.
3B PROMPT_HEAVY — COMPLETE: repeated 0-read result path; changing dynamic values required prompt mutation.
3C HYBRID dynamic mutation — NEXT: change the same generation/marker in GitHub only, then collect 2 clean samples.
3D POINTER_ONLY baseline — after 3C: collect 2 clean samples.

## Stage 4 — Discriminating failure tests
Test only faults that distinguish the candidates:
- missing GitHub entrypoint;
- stale deployed prompt;
- repeated dynamic-state churn;
- scheduler write accepted but later wake absent.

## Stage 5 — Convergence
Build the comparison table, choose the smallest design that preserves correctness and recovery, then remove unnecessary rules/files.

## Stage 6 — Final validation
Run 3 clean end-to-end wakes plus 1 recovery wake. Produce the final prompt, repo schema, recovery protocol, rollout method, and rejected-alternative record.

## Stop rules
Do not repeat a converged sample unless an anomaly requires it. Do not change more than one primary experimental variable at a time.
