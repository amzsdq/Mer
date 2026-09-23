# Direct Successor CAS Test Protocol

Status: IMMUTABLE_SHADOW_CANDIDATE
Primary variable: ownership transfer trigger only.

## Fixed conditions
- same recurring automation
- every wake performs exactly one verified +14m pre-arm before substantive work
- substantive writes require fresh exact owner+generation
- immutable evidence remains allowed to SHADOW
- no lease timeout or heartbeat is introduced in this test

## Candidate transfer
1. Successor wakes and pre-arms its own successor +14m.
2. Successor writes immutable READY evidence.
3. Successor fresh-reads ownership record and blob SHA.
4. Successor attempts exactly one conditional update: generation := generation+1; active_invocation_id := successor; predecessor_invocation_id := prior owner.
5. Winner fresh-reads and confirms exact owner+generation before authoritative work.
6. Predecessor fresh-reads before its next owner-only side effect; if generation changed, it performs no further authoritative work and closes as SUCCESSOR_HANDOFF_COMPLETE.
7. Any racing successor whose conditional update loses remains SHADOW and must not retry unchanged in the same generation.

## Required evidence
- predecessor id/generation
- successor wake and READY commit/server evidence
- ownership SHA before attempt
- CAS winner and resulting generation
- predecessor fence check after transfer
- first successor authoritative-work evidence
- duplicate authoritative side-effect count
- scheduler prearm count per invocation
- scheduler conflicts
- handoff gap

## Pass
Exactly one takeover winner; predecessor performs zero authoritative side effects after generation change; successor resumes useful work; one pre-arm per invocation; no scheduler conflict.

## Reject/revise
- duplicate authoritative side effect => reject current fencing implementation
- more than one ownership winner => reject CAS implementation
- a legitimate non-successor wake can seize ownership => add explicit successor admissibility token rather than time lease first
- scheduler collision caused by one-prearm-per-invocation => revise scheduler lane separately, do not conflate with ownership transfer

## Comparator
Lease-expiry takeover remains comparator. Direct successor CAS is preferred if reliability is equivalent because it removes expiry calibration, heartbeat state, and recovery delay.
