# O1 OWNER LOSS CALIBRATION 002 — PIGGYBACK VALIDITY RULES

Status: IMMUTABLE_SHADOW_ANALYSIS

Not every useful-work commit may count as owner liveness. A piggyback checkpoint is valid only when:
- it is published by the current authoritative owner invocation;
- it identifies the current generation;
- the owner performed a fresh ownership read before the checkpoint boundary;
- the checkpoint is already required by the work protocol rather than created solely to manufacture heartbeat traffic;
- its GitHub server timestamp is directly observable.

A SHADOW's frequent evidence writes cannot renew an OWNER lease. Current calibration 002 therefore supplies cadence/overhead insight only, not owner liveness samples.

This distinction prevents an accidental bug where any repository activity keeps a dead owner's lease alive.
