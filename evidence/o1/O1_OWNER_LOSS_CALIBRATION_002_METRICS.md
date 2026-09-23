# O1 OWNER LOSS CALIBRATION 002 — METRICS DEFINITION

Status: IMMUTABLE_SHADOW_ANALYSIS

For future canaries, record separately:

- detection_latency_sec: server timestamp at takeover eligibility minus last valid owner liveness server timestamp.
- takeover_latency_sec: successful ownership CAS server timestamp minus eligibility observation timestamp where directly observable.
- resume_latency_sec: first resumed useful-work evidence server timestamp minus successful takeover timestamp.
- recovery_total_sec: first resumed useful-work evidence minus last valid owner liveness evidence.
- false_takeover: takeover attempted while owner remains within declared valid liveness window.
- duplicate_authoritative_effects: count.
- scheduler_writer_conflicts: count.
- control_write_count: renewal/recovery-only writes.
- useful_write_count: writes that directly advance research/work product.

Do not collapse WRITE_OK/STATE_OK/WAKE_OK/WORK_OK into one success metric. A takeover can be state-correct but fail to resume useful work.
