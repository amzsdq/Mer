# O1 Passive Timing Observation — 2026-09-24 07:32 KST

## Scope
Non-authoritative SHADOW observation for `H-O1-OWNER-LOSS-RECOVERY` calibration. No ownership or program-state mutation is performed by this record.

## Authoritative state observed
- `status/program.json`: stage `O1_CONTROLLED_OVERLAP_BASELINE`; next step `O1_OWNER_LOSS_RECOVERY_TEST_DESIGN_AND_EXPIRY_CALIBRATION`.
- `spec/execution.json`: expiry threshold remains `UNSET_REQUIRES_CALIBRATION`; minimum genuine renewal gaps = 5; active experiment still requires genuine owner-renewal gap distribution.
- `control/ownership.json`: generation 2, active invocation `MER-20260924T060808+0900-O1BOOT001`, `owner_renewal_seq=1`.

## Search performed
Repository code/evidence search for comparable passive timing fields (`PASSIVE_TIMING_OBSERVATION`, actual wake, READY, scheduled_for, observed_at) returned no additional indexed comparable samples in this wake.

## Observation
This wake itself demonstrates continued scheduler liveness but does not create a valid OWNER-renewal sample: the current invocation is SHADOW under generation 2. Scheduler wake success must remain distinct from owner liveness and cannot be substituted for a renewal timestamp.

## Decision
`KEEP_PASSIVE_TIMING_OBSERVATION`.

Do not derive a time-only expiry from the +840s scheduler pre-arm or from the single bootstrap renewal. The evidence set remains insufficient to separate healthy-owner activity from wake/READY delay tails.

## Next useful unit
Continue collecting directly observable actual-wake/READY or equivalent liveness timing samples. Re-evaluate provisional time-only expiry only after at least 5 comparable observations. If the distributions cannot be separated, reject time-only expiry and test an explicit renewable liveness signal instead.
