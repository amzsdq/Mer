# O1 OWNER LOSS CALIBRATION 002 — DECISION MATRIX

Status: IMMUTABLE_SHADOW_ANALYSIS

| Mechanism | Unplanned owner-loss liveness | False takeover control | Extra steady-state writes | Safety independent of detector | Current verdict |
|---|---|---|---|---|---|
| Owner-mediated only | No | N/A | Low | Yes | REJECT for recovery |
| Immediate SHADOW CAS | Fast | Poor | Low | Requires fence | REJECT |
| Dedicated renewal lease + CAS | Yes | Calibratable | Medium/High | Yes with generation fence | KEEP/TEST |
| Piggyback required checkpoints + CAS | Potentially | Depends on bounded checkpoint cadence | Low | Yes with generation fence | COMPETING CANDIDATE |
| Predelegated successor token | Partial/planned loss only | Good for declared successor | Low | Yes with fence | Comparator, not full recovery |

## Selection rule
Prefer piggybacked renewal if Mer can prove/measure a bounded normal checkpoint cadence that supplies equivalent liveness evidence. Otherwise dedicated renewal lease remains the controlled baseline for owner-loss recovery.

## Next discriminating measurement
Across real owner wakes, measure server timestamps of already-required durable checkpoints before introducing dedicated heartbeat writes. Determine whether their maximum observed gap plus close/runtime bounds is sufficiently stable to act as renewal evidence. This changes no authoritative state and preserves the one-primary-variable rule.
