# Mer Master Plan v2

## Rule
Each wake follows the authoritative program state in `status/program.json`. It does not choose a new experiment ad hoc. New ideas go to backlog unless they invalidate the current test.

Dynamic runtime authority is singular:
- `status/program.json` owns `current_stage`, `next_step`, and `active_execution`.
- `control/active.json` is a static bootstrap pointer and must not duplicate changing stage/next fields.
- `spec/execution.json` is authoritative only while `status/program.json.active_execution` explicitly points to it.

## Stage 0 — Reconnaissance — COMPLETE
Compared RRULEF, tEST, workwork and external controller/durable-workflow patterns.

## Stage 1 — Instruction authority — COMPLETE
Tested direct prompt instruction, explicit delegation to GitHub, and pointer-only behavior.

## Stage 2 — Hybrid failure handling — COMPLETE
Tested missing reference, malformed state, generation mismatch, semantic conflict, and prompt drift.

## Stage 3 — Normal-path boundary cost — COMPLETE
- HYBRID baseline: clean samples demonstrated GitHub-owned dynamic state with bounded bootstrap reads.
- PROMPT_HEAVY: 0-read result path demonstrated; changing dynamic values required deployed prompt mutation.
- HYBRID dynamic mutation: GitHub-only dynamic changes demonstrated without deployed prompt mutation.
- POINTER_ONLY: dynamic-state freshness preserved but recovery from loss of the sole entrypoint was weaker.

## Stage 4 — Discriminating failure tests — COMPLETE
Tested the candidate-distinguishing failures needed for the final decision, including missing entrypoint, stale/deployed prompt drift, dynamic churn, and the separation between scheduler acceptance/live state/later wake.

## Stage 5 — Convergence — COMPLETE
Selected the smallest supported design that preserved correctness and recovery:
**HYBRID stable kernel + GitHub dynamic brain**.

## Stage 6 — Final validation — COMPLETE
Validated:
- clean end-to-end wakes: 3/3 PASS
- missing-entrypoint recovery: 1/1 PASS

Produced:
- final prompt
- final repository schema
- recovery protocol
- rollout method
- rejected-alternative record

## Finalization rule
Completion must not depend on synchronizing multiple changing files.

Authoritative completion is one state transition in `status/program.json`:
- `current_stage = COMPLETE`
- `next_step = NONE`
- `active_execution = null`

Other documents and archive cleanup are non-authoritative follow-up work. If cleanup fails, runtime truth remains unambiguous.

## Wake utilization rule
A wake is a work session, not a one-step callback.

Initial operating target during experimentation was about 600 seconds of useful work per normal wake. It was an empirical starting point, not a permanent invariant. Never sleep, pad, or invent work to hit a target.

## Stop rules
The research program is COMPLETE. Do not start a new experiment unless the program state is explicitly reopened through a new authoritative program transition.
