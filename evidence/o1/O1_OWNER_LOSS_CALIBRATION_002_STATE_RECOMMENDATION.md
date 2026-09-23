# O1 OWNER LOSS CALIBRATION 002 — OWNER-ADMISSIBLE STATE RECOMMENDATION

Status: IMMUTABLE_SHADOW_RECOMMENDATION

## Recommended next authoritative transition
Do not set an expiry threshold yet. Keep H-O1-OWNER-LOSS-RECOVERY as TESTABLE_CANDIDATE / KEEP-REVISED.

When a valid OWNER next controls generation, recommended next step is:
`O1_MEASURE_REAL_OWNER_REQUIRED_CHECKPOINT_CADENCE_FOR_PIGGYBACKED_RENEWAL`

Reason:
- current SHADOW sample shows natural durable writes can be frequent but is not representative;
- dedicated heartbeat adds control overhead and should not be introduced until necessary;
- real OWNER required-checkpoint cadence can discriminate piggyback vs dedicated renewal with less complexity;
- threshold G remains UNSET until cadence/lateness is measured.

## Recommended experiment state (owner may adopt after fresh authority validation)
- primary_variable remains OWNER_LOSS_RECOVERY_MECHANISM;
- scheduler recurrence unchanged;
- no takeover during cadence measurement;
- collect server `created_at` for already-required OWNER checkpoints;
- classify whether cadence has a usable upper bound under normal work;
- only then select piggyback or dedicated renewal baseline.

This recommendation does not mutate `status/program.json` or `spec/execution.json` because this invocation is SHADOW.
