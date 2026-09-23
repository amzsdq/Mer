# O1 OWNER LOSS CALIBRATION 002 — NEXT WAKE INPUT

Status: IMMUTABLE_SHADOW_HANDOFF

Next invocation should fresh-read authoritative state first. If ownership is still the old generation-1 predecessor, remain SHADOW and do not mutate scheduler/program state.

Highest-value next evidence:
1. If a valid OWNER exists, measure natural required-checkpoint cadence using server timestamps before adding heartbeat writes.
2. If still no valid OWNER, continue refining/test-preparing recovery without self-promotion; do not select expiry from SHADOW cadence.
3. Preserve `H-O1-OWNER-LOSS-RECOVERY` as KEEP_REVISED until real owner cadence + false-takeover canary exist.
4. Explicitly test scheduler cross-system fencing before promotion.

Prepared artifacts from calibration 002 include canary spec, test vectors, metrics, threshold procedure, state machine, rejection criteria, and minimal-write comparator.
