# O1 OWNER LOSS CALIBRATION 002 — OPEN QUESTIONS

Status: IMMUTABLE_SHADOW_ANALYSIS

Remaining empirical questions, ordered by leverage:

1. Do real OWNER required checkpoints have a bounded enough server-timestamp cadence to eliminate dedicated heartbeat writes?
2. What normal positive lateness distribution exists relative to that cadence?
3. Can the scheduler cross-system TOCTOU race produce an actual conflicting write after generation takeover?
4. Does prearmed overlap place a prepared SHADOW close enough to expiry to make recovery latency acceptable?
5. What is the control-overhead delta between piggyback and dedicated renewal?
6. Can scheduler writes be made sufficiently convergent/idempotent without another authority domain?

These are empirical; no further paper design should be promoted as truth before tests.
