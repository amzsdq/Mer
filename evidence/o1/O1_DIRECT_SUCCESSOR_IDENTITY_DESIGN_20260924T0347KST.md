# O1 Direct Successor Identity Design

Status: SHADOW_IMMUTABLE_EVIDENCE
Date: 2026-09-24
Hypothesis: H-O1-OWNER-LOSS-RECOVERY
Primary variable: OWNER_LOSS_RECOVERY_MECHANISM

## Observation
The current ownership record has generation=1 and intended_successor_invocation_id=NEXT_WAKE_SHADOW_PENDING_ID. That placeholder cannot distinguish the intended +14m successor from a duplicate, delayed, or replayed invocation.

## Minimal non-time-based candidate
A direct successor CAS can replace lease-expiry only if the predecessor durably mints a single-use successor token before handoff. Minimal fields:

- generation: current authoritative generation
- active_invocation_id: current owner
- successor_token: opaque unique value minted for exactly one next baton
- successor_for_generation: current generation
- successor_state: ARMED | CONSUMED
- successor_armed_at: GitHub-server-backed durable creation/update evidence

Candidate acquisition predicate:
1. fresh-read ownership;
2. successor_state == ARMED;
3. successor_for_generation == current generation;
4. candidate presents exact successor_token delivered by the baton context;
5. one SHA-guarded update atomically increments generation, names candidate active_invocation_id, and marks old token CONSUMED/replaces it with the candidate's newly armed next token.

## Safety properties to test
T1 LIVE_PREDECESSOR: intended successor CAS wins once; predecessor's next authoritative write must fail fresh owner+generation fence.
T2 LOST_PREDECESSOR: intended successor CAS wins without waiting for a lease timeout and resumes useful work.
T3 DUPLICATE_RACE: two candidates presenting the same token race from the same blob SHA; exactly one update may succeed, loser must fresh-read and remain SHADOW.
T4 REPLAY: a consumed token from generation N must fail after generation advances to N+1.
T5 WRONG_SUCCESSOR: a wake lacking the exact token must not acquire ownership merely because its dispatch time is near +14m.

## Comparator decision rule
Prefer DIRECT_SINGLE_USE_SUCCESSOR_CAS over LEASE_EXPIRY only if T1-T5 pass with zero duplicate authoritative side effects and the baton can actually deliver the opaque token to the intended successor. If the automation platform cannot carry a per-baton token into the next invocation without unsafe prompt mutation or an additional coordination channel, reject this candidate as operationally unavailable and retain/calibrate lease recovery.

## External-prior translation
GitHub Contents updates require the current blob SHA when replacing a file; this provides an optimistic-concurrency conflict primitive for the single ownership record, not a multi-file transaction. Kubernetes Lease remains the comparator for crash recovery when explicit successor identity cannot be delivered; its leader-election design uses a shared Lease and takeover after missed renewal.

## Current verdict
REVISE / TESTABLE_ONLY. Do not promote direct CAS yet. The next discriminating question is not timeout calibration; it is whether the baton path can securely and durably bind one opaque successor token to exactly one next invocation.
