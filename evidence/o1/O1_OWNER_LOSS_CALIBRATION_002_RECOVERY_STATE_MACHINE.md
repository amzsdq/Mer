# O1 OWNER LOSS CALIBRATION 002 — RECOVERY STATE MACHINE CANDIDATE

Status: IMMUTABLE_SHADOW_SYNTHESIS

States:
- OWNER_HEALTHY
- SHADOW_READY
- RECOVERY_ELIGIBLE
- TAKEOVER_CAS
- NEW_OWNER_VERIFY
- RESUMED_WORK
- RECOVERY_FAILED

Transitions:
OWNER_HEALTHY -> SHADOW_READY: successor wakes/prepares.
SHADOW_READY -> OWNER_HEALTHY: normal owner-mediated transfer path proceeds / no recovery needed.
SHADOW_READY -> RECOVERY_ELIGIBLE: calibrated server-observed non-renewal threshold crossed.
RECOVERY_ELIGIBLE -> TAKEOVER_CAS: fresh ownership read still names expired generation.
TAKEOVER_CAS -> NEW_OWNER_VERIFY: SHA/CAS generation+1 succeeds.
TAKEOVER_CAS -> SHADOW_READY: CAS loses/conflicts; fresh-read and remain non-authoritative.
NEW_OWNER_VERIFY -> RESUMED_WORK: scheduler/state verified and bounded useful work resumes.
Any ambiguous authority/server evidence -> RECOVERY_FAILED/fail-closed for authoritative path.

Normal healthy transfer remains outside recovery CAS path. This state machine is candidate documentation only, not authoritative runtime state.
