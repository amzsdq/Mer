# O1 Calibration Escape Synthesis — 2026-09-24 07:47 KST

Status: IMMUTABLE_SHADOW_DIAGNOSIS
Authority: none; does not mutate program/execution/ownership.

## Finding
Repeated passive timing observations are not producing the required genuine-owner renewal distribution because generation 2 is bound to a terminated ephemeral invocation. Continuing to collect scheduler wake observations alone cannot satisfy the active execution's `minimum_genuine_renewal_gaps=5`; scheduler liveness is not owner liveness.

A previously recorded candidate already removes this circular dependency: `O1_DIRECT_SUCCESSOR_CAS_TEST_PROTOCOL_TURN4.md`. It transfers ownership by a one-shot SHA-guarded generation CAS from a READY successor, with predecessor fencing on fresh-read. Its comparator is lease-expiry takeover, and it explicitly prefers direct CAS if reliability is equivalent because expiry calibration, heartbeat state, and recovery delay disappear.

## New synthesis
The direct-successor candidate should be tested before spending more samples on time-only expiry calibration, but only after successor admissibility is explicit. Otherwise any unrelated wake could attempt takeover from a dead predecessor.

Minimal admissibility requirement:
1. successor has performed this wake's verified scheduler pre-arm;
2. successor has reconstructed current program/checkpoint and written immutable READY evidence;
3. durable state names or cryptographically/uniquely binds the intended successor eligibility for the current generation, OR an experimentally declared one-shot recovery admission rule explicitly permits the current READY successor;
4. successor fresh-reads ownership SHA immediately before exactly one CAS;
5. CAS increments generation exactly once; loser stays SHADOW;
6. winner fresh-reads exact owner+generation before authoritative work.

Do not use elapsed time as an eligibility substitute in this test. That would compound the transfer-trigger variable with an uncalibrated expiry variable.

## Model update
- Repeated passive scheduler observations without owner-liveness samples: REJECT as the immediate next experiment; they cannot close the active calibration gate by themselves.
- Time-only expiry: KEEP only as comparator, not primary path.
- Direct READY-successor CAS with explicit admissibility: PROMOTE TO NEXT TESTABLE CANDIDATE, subject to authoritative program revision by a valid owner/recovery transition.

## Expected value
If direct CAS passes, O1 can validate overlap handoff without first solving lease expiry. Owner-loss recovery can then be tested separately as a recovery problem rather than being made a prerequisite for ordinary handoff. This reduces coupled state and follows the simplicity objective.

## Safety
No authoritative state changed in this diagnosis. Current generation/owner remains authoritative until a separately admitted transition occurs.
