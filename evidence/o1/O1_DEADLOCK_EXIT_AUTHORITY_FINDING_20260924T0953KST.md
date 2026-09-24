# O1 Deadlock Exit Authority Finding

## Observation
Authoritative `spec/execution.json` still requires at least five `GENUINE_OWNER_RENEWAL_GAP` samples before deriving an expiry candidate. `control/ownership.json` still names generation-2 invocation `MER-20260924T060808+0900-O1BOOT001` as OWNER with `owner_renewal_seq=1`. Current wakes are SHADOW successors and cannot create genuine renewal samples for that dead invocation.

## New discriminating finding
The blocker is no longer a measurement problem. It is an authority-transition problem. More passive SHADOW observations cannot satisfy the declared primary variable, and substituting scheduler wakes for OWNER renewals would corrupt the experiment.

The smallest safe exit is not to guess an expiry. It is an explicit, durable, single-use program-authorized recovery epoch bound to the currently stuck generation. That authorization must be declared by the authoritative program/execution state before any SHADOW attempts ownership CAS. The CAS must fresh-read the authority record, increment generation, and preserve per-side-effect fencing. After a live OWNER exists again, genuine renewal-gap calibration can resume without circular dependence.

## Safety consequence
This SHADOW invocation MUST NOT edit `status/program.json`, `spec/execution.json`, or `control/ownership.json` under the current rules because `no_ad_hoc_self_promotion=true` and the prior bootstrap exception is consumed. It therefore records this immutable evidence only.

## Model update
- REJECT: more unchanged passive timing samples as the immediate path.
- REJECT: treating scheduler wake/pre-arm as OWNER renewal.
- KEEP: generation fencing and fresh-read optimistic concurrency.
- REVISE: O1 ordering so an explicit single-use recovery authorization precedes renewal calibration when the current OWNER cannot renew.

## Required authoritative next action
Revise the active experiment to authorize exactly one recovery epoch for generation 2, run a gen2->gen3 recovery canary, then resume genuine OWNER renewal sampling from the recovered live OWNER. If such authorization is not granted, the correct state is an explicit authority blocker rather than repeated calibration attempts.
