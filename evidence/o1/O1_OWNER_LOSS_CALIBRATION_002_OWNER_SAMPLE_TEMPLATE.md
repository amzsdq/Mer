# O1 OWNER LOSS CALIBRATION 002 — REAL OWNER SAMPLE TEMPLATE

Status: PREPARED_NOT_AUTHORIZED

For each future real OWNER wake, capture:
- owner_invocation_id
- generation
- START_MARKER commit + server created_at
- each already-required durable checkpoint commit + server created_at
- checkpoint purpose (useful/control)
- END_MARKER commit + server created_at
- maximum gap between valid owner liveness checkpoints
- whether any substantial unit legitimately exceeded that gap
- scheduler WRITE_OK/STATE_OK/WAKE_OK/WORK_OK

Do not add heartbeat writes during the first measurement phase. The purpose is to observe natural required-checkpoint cadence B. After several real owner samples, decide whether B is sufficiently bounded for piggybacked renewal.
