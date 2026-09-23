# O1 OWNER LOSS CALIBRATION 002 — EVIDENCE MINIMIZATION

Status: IMMUTABLE_SHADOW_ANALYSIS

Calibration 002 produced many small artifacts because the new duration gate required continued useful work and frequent server-observable boundaries. This is acceptable for diagnosis but not desirable as steady-state design.

Future protocol should minimize required artifacts to:
- START/END work markers for duration authority;
- already-required useful checkpoints/evidence;
- READY/transfer/takeover evidence for handoff;
- exceptional fault evidence.

Do not require a standalone file for every internal reasoning boundary. Where server-time observation is needed, reuse a required durable artifact when possible. This reduces GitHub write overhead and repository noise without weakening evidence quality.
