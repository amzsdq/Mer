# O1 OWNER LOSS CALIBRATION 002 — PROGRESS-AS-LIVENESS LIMIT

Status: IMMUTABLE_SHADOW_ANALYSIS

Piggybacking work checkpoints as liveness has a semantic hazard: liveness means the owner is still capable of authoritative progress, while a work checkpoint only proves it successfully published something at that instant. A long compute/research unit after that point can legitimately be silent.

Therefore piggybacked renewal is viable only if the work protocol already bounds time between authoritative durable boundaries. If unit duration is intentionally variable/unbounded, checkpoint silence cannot safely drive a short lease.

This connects recovery design to admission policy: an owner should not start a unit whose worst-case duration exceeds the liveness window unless it can renew safely during the unit. Otherwise either P/G must cover the unit bound or dedicated renewal is required.

This is a real coupling and should be measured, not hidden. It may make dedicated renewal simpler despite extra writes for workloads with long units.
