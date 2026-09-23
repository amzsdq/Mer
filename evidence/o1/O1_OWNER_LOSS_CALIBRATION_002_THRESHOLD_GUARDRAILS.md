# O1 OWNER LOSS CALIBRATION 002 — THRESHOLD GUARDRAILS

Status: IMMUTABLE_SHADOW_ANALYSIS

## Guardrails before selecting P or G
1. Do not derive P from this SHADOW wake's fast write cadence; owner workload may differ.
2. Do not derive G from the 7–8 s transport sample; that measured write spacing, not scheduling lateness.
3. Do not derive G from the ~1 h cross-wake interval; recurrence dominates it.
4. Do not use `control/ownership.json.updated_at`, `owner_started_at`, or other model-authored strings for expiry.
5. Do not let timeout correctness carry safety; generation fencing remains mandatory.
6. Do not vary renewal cadence P and grace G in the same discriminating sample unless explicitly declared compound.

## Minimum evidence to set a provisional threshold
- server-timestamped valid renewal/checkpoint samples from real OWNER wakes,
- declared nominal expected cadence,
- observed positive lateness against that cadence,
- at least one normal-delay false-takeover canary before intentional loss.

## Why this matters
Without these guardrails a small empirical sample can produce an apparently precise expiry that is actually measuring the wrong clock/process. The current threshold remains UNSET by design.
