# O1 OWNER LOSS CALIBRATION 002 — RECOVERY LATENCY BOUND

Status: IMMUTABLE_SHADOW_ANALYSIS

With renewal/checkpoint bound P and grace G, takeover eligibility alone is bounded by approximately P+G after the last valid liveness epoch. Actual resumed-work latency additionally includes candidate wake/observation delay, CAS time, verification, and resume overhead.

Therefore claiming 'recovery bounded by P+G' would be false unless candidate observation itself is guaranteed within that window. Under hourly recurrence, observation delay can dominate.

Implication: owner-loss recovery must be evaluated jointly with the active overlap/wake strategy. A correct lease may still provide poor duty cycle if no candidate wakes near expiry. The existing prearmed SHADOW successor is therefore important for liveness latency, while lease/CAS supplies authority recovery.

Do not fold scheduler timing into lease threshold calibration; measure it separately as wake/observation latency.
