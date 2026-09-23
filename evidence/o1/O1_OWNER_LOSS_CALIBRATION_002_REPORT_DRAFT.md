# O1 OWNER LOSS CALIBRATION 002 — REPORT DATA

Status: IMMUTABLE_SHADOW_EVIDENCE

STAGE: O1_CONTROLLED_OVERLAP_BASELINE
STEP: O1_OWNER_LOSS_RECOVERY_TEST_DESIGN_AND_EXPIRY_CALIBRATION
HYPOTHESIS: H-O1-OWNER-LOSS-RECOVERY
RESULT: KEEP_REVISED; calibration semantics narrowed; no threshold promoted.
GATE: O1 clean handoffs remains 0/3; calibration is not a clean handoff.
WRITE_OK: YES for immutable SHADOW evidence only.
STATE_OK: YES, authoritative state reconstructed and left unchanged by SHADOW.
WAKE_OK: YES, current wake executed.
WORK_OK: YES for substantive non-authoritative research; not authoritative resumed-work after takeover.
NEXT: real OWNER required-checkpoint cadence measurement, then choose piggyback vs dedicated renewal, then false-takeover canary.

Duration START authority: commit `892eaf3203c1a6c0497f626cb3ba364d427bc4aa` at 17:07:02Z. END must not be written until server evidence reaches >=600 s at a safe boundary.
