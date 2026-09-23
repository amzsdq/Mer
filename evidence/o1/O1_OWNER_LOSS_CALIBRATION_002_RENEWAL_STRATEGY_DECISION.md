# O1 OWNER LOSS CALIBRATION 002 — RENEWAL STRATEGY DECISION RULE

Status: IMMUTABLE_SHADOW_SYNTHESIS

Choose between piggybacked and dedicated renewal using one measurable criterion:

- If normal OWNER protocol has a defensible upper bound B on time between valid required durable checkpoints, use those checkpoints as renewal evidence and calibrate expiry above B plus observed server/provider lateness.
- If no such B exists, use dedicated renewal independent of workload units.

Do not mix both mechanisms in the first canary. Mixing them would obscure which signal prevented/caused takeover.

This reduces the next OWNER task to a concrete measurement: determine whether B exists under real work. It avoids prematurely choosing a heartbeat cadence.
