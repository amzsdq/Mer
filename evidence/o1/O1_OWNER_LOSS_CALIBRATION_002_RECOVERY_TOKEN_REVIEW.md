# O1 OWNER LOSS CALIBRATION 002 — PREDELEGATED TOKEN REVIEW

Status: IMMUTABLE_SHADOW_ANALYSIS

A predelegated successor token could simplify planned handoff: owner names a specific successor and grants conditional generation transfer. But for unplanned owner loss, the condition still needs an observable trigger. If trigger is timeout/non-renewal, the token does not eliminate failure detection; it only narrows eligible contender identity.

Given current overlap already has an intended successor field, contender narrowing may be useful later, but it should not be confused with owner-loss detection. Adding token semantics now would change another variable without removing the lease problem.

Decision: keep as later optimization, not current recovery mechanism.
