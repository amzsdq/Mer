# O1 Baton Direct-CAS Candidate

Status: IMMUTABLE_SHADOW_EVIDENCE

## Observation
The new runtime guarantees that every invocation is itself a deliberately pre-armed successor wake (+14m). Therefore owner-loss recovery no longer necessarily needs a separate time-expiry lease before transfer.

## Simpler candidate
At successor wake, after the mandatory scheduler pre-arm, the successor fresh-reads control/ownership.json and attempts one SHA-guarded ownership update that increments generation and names itself owner. If two invocations race, only one matching-SHA write can win. The predecessor remains physically alive if still running, but its next authoritative side effect must fresh-read owner+generation and will fail the fence after takeover.

## Why this may dominate lease expiry
- No model or wall-clock expiry decision.
- No lease-duration calibration or heartbeat traffic.
- Owner crash before handoff is recovered by the already-scheduled successor.
- Normal overlap and owner-loss use the same transfer primitive.
- Generation fencing remains mandatory because election/coordination alone cannot physically stop a stale predecessor.

## Discriminating tests
1. Normal predecessor alive: successor CAS generation+1; predecessor fresh fence must reject subsequent owner-only mutation.
2. Predecessor dead: successor CAS generation+1 and resumes work without waiting for expiry.
3. Duplicate/racing successor: exactly one CAS winner; loser remains SHADOW.
4. Verify scheduler lane remains independent: each wake performs exactly one +14m pre-arm, with no post-prearm scheduler mutation.

## Promotion rule
Prefer DIRECT_SUCCESSOR_CAS over lease-expiry recovery if the four tests pass with zero duplicate authoritative side effects and zero scheduler conflicts. Retain lease expiry only if a real wake can occur that is not an intended successor and therefore needs an additional admissibility test.

## Conflict discovered
spec/execution.json still contains a side_effect_fence_rule string that groups scheduler side effects with owner-only side effects, while the new scheduler_writer_rule explicitly permits every invocation one wake-start pre-arm. This is stale policy text and should be replaced by the OWNER when authoritative state is next mutated; scheduler pre-arm must be fenced by the per-wake one-write rule, while substantive shared-state mutations remain owner+generation fenced.
