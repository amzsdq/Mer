# O1 OWNER LOSS CALIBRATION 002 — RESEARCH SUMMARY

Status: IMMUTABLE_SHADOW_SYNTHESIS

Calibration 002 materially narrowed H-O1-OWNER-LOSS-RECOVERY:

- Cross-wake server evidence measured 60m56s START-to-START and 59m43s prior-END-to-current-START under hourly recurrence; these are not lease jitter samples.
- Expiry must be defined against a declared valid owner renewal/checkpoint cadence, not arbitrary write latency or model timestamps.
- Safety and liveness are separated: timeout admits takeover attempts; generation CAS/fencing protects authority.
- Healthy handoff remains owner-mediated; lease is fallback only.
- Piggybacking already-required OWNER checkpoints is preferred if a bounded cadence exists; dedicated heartbeat is fallback.
- READY can double as fresh server-time observation, avoiding clock-only writes.
- Scheduler remains a cross-system fencing risk and must be explicitly canary-tested.
- Recovery latency must be decomposed into detection, observation/wake, CAS, and resume components.
- No numerical expiry threshold is justified yet; real OWNER cadence samples are the next discriminating evidence.

Current hypothesis verdict: KEEP_REVISED, not promoted.
