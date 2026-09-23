# O1 OWNER LOSS CALIBRATION 002 — AUTHORITY RECORD REVISION CANDIDATE

Status: PREPARED_NOT_AUTHORIZED

If lease recovery is promoted later, avoid storing authoritative expiry as a model-authored timestamp inside `control/ownership.json`.

Minimal ownership record should continue to hold:
- active_invocation_id
- generation
- status
- intended/predecessor identifiers as needed
- authority rule

Liveness should be referenced by immutable server-timestamped evidence keyed to owner+generation, not by `updated_at` or `expires_at` strings written by the model.

This separation keeps ownership identity and failure-detection evidence distinct and avoids two competing clocks in one record.
