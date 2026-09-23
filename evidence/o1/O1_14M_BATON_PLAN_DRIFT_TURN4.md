# O1 14m Baton Plan Drift Audit

Status: IMMUTABLE_SHADOW_EVIDENCE

The active program/execution state now declares WAKE_START_FIXED_PREARM_BATON, no work-duration target, and one scheduler pre-arm per invocation. MASTER_PLAN Stage O1 still describes the superseded 600s/420s baseline and exactly one ACTIVE_OWNER scheduler writer. This is documentation/plan drift, not authority for reverting the active runtime.

Required owner-side reconciliation at the next authoritative mutation:
- replace the old O1 baseline timing description with the active +14m wake-start baton candidate;
- change the clean-sample scheduler criterion from one owner-only scheduler writer to one verified pre-arm per invocation and zero extra scheduler writes;
- retain generation/CAS substantive ownership fencing and zero duplicate authoritative side effects;
- add DIRECT_SUCCESSOR_CAS as the simpler recovery comparator to lease-expiry takeover.

Do not count this wake as a clean handoff until a successor actually wakes, wins/accepts generation transfer, and resumes authoritative work.
