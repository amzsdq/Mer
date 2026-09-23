# O1 Owner-Loss Recovery — Turn 9

Observed authoritative state:
- runtime candidate: WAKE_START_FIXED_PREARM_BATON, +840s, no work-duration target
- recovery expiry_threshold: UNSET_REQUIRES_CALIBRATION
- ownership record remains generation 1, active_invocation_id MER-20260923T215700+0900-O1S001, with no renewable liveness field

Decision:
1. Do not perform ad-hoc takeover. Existing recovery constraints explicitly forbid it.
2. Do not derive expiry from owner_started_at or model-written updated_at; expiry requires GitHub-server timestamp evidence from genuine OWNER renewals.
3. Minimal calibration signal remains a generation-scoped owner_renewal_seq in the single ownership authority record. Each valid OWNER renewal increments the sequence at a genuine safe checkpoint; the accepted GitHub commit timestamp is the time source.
4. Collect >=5 consecutive genuine renewal gaps before proposing expiry. Synthetic rapid writes are excluded.
5. Candidate expiry must then pass, in order: healthy-owner false-takeover canary; intentional owner-loss recovery; two-candidate race; stale-owner fencing; recovery-latency measurement.
6. Dedicated heartbeat is rejected unless natural safe-checkpoint renewal gaps prove too sparse/variable for acceptable recovery latency.

Current classification: TESTABLE_CANDIDATE, not promoted.
Current blocker: no valid active OWNER exists to generate genuine renewal-gap samples. This is an experimental liveness deadlock: calibration requires an OWNER, while safe recovery of the stale OWNER requires calibrated recovery evidence.

Next research action: design a one-time bootstrap/recovery canary that breaks this circular dependency without silently promoting an uncalibrated lease rule. The bootstrap must be explicitly experimental, generation-fenced, optimistic-concurrency guarded, and reversible; it must not be generalized as the production takeover rule until the calibration gates pass.
