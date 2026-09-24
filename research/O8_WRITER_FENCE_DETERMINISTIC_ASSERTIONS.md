# O8 Writer-Fence Deterministic Assertions

Status: ACTIVE TEST ORACLE
Hypothesis: `H-O8-SCHEDULER-WRITER-FENCE`

These assertions make the B-arm decision falsifiable without relying on prose interpretation.

## Normal handoff assertions
Given source generation G with one OPEN epoch targeting G+1:

WF-N0 ATTRIBUTION_COMPLETE
- The scheduler mutation must have a protocol-complete immutable pre-mutation intent containing invocation, generation/role, epoch, reason, intended DTSTART, full VEVENT, timing/enabled state, representation path, shadow-write count and legal ownership/CAS evidence.
- A post-hoc attribution addendum preserves honesty but cannot satisfy WF-N0 for CLEAN qualification.

WF-N1 SHADOW_WRITE_ZERO
- Before a contender wins fresh-SHA CAS, scheduler-write intent count for that contender MUST equal 0.

WF-N2 ONE_CAS_WINNER
- Across contenders bound to the same epoch, successful ownership transition count MUST equal 1. Losing contenders remain non-authoritative.

WF-N3 POST_CAS_WRITE_ONE
- CAS winner may issue exactly one normal scheduler-write intent/result pair after ownership acquisition; generation/role must match the committed owner.

WF-N4 EXACT_STATE
- Post-CAS owner result must match canonical ID, enabled=true, exact_schedule, RRULE:FREQ=HOURLY, and exact intended absolute DTSTART.

WF-N5 WORK_RESUMES
- New owner must produce durable genuine-work progress after CAS/prearm.

WF-N6 NO_DUPLICATE_AUTHORITY
- No authoritative shared-state mutation may be accepted from source generation G after G+1 CAS commit.

## Stability assertions
WF-S1 NO_UNATTRIBUTED_OVERWRITE
- Between exact STATE_OK and intended target, any changed live DTSTART must have matching durable writer attribution or be classified instrumentation gap; unexplained overwrite fails CLEAN.

WF-S2 WAKE_DISTINCT_FROM_STATE
- WRITE_OK+STATE_OK never implies WAKE_OK. WAKE_OK requires actual later dispatch attributable to intended recurring state or documented provider-equivalent recurrence.
- Intentionally superseded target => `SUPERSEDED_NOT_CLEAN`.

WF-S3 WORK_DISTINCT_FROM_WAKE
- WAKE_OK never implies WORK_OK. WORK_OK requires durable substantive progress in resumed invocation.

WF-S4 TARGET_SURVIVES_TO_DISPATCH
- CLEAN requires exact intended target to remain authoritative until qualifying dispatch. Intentional corrective/experimental rewrite before target cannot count CLEAN.

## Owner-loss adverse assertions
WF-A1 EXPLICIT_SINGLE_USE_AUTH
- Recovery takeover requires explicit durable single-use authorization scoped to exact source generation/owner/target generation. Elapsed time alone is insufficient.

WF-A2 SHADOW_STILL_NO_WRITE
- Under owner loss, contender scheduler-write count remains 0 until legal recovery CAS succeeds.

WF-A3 RECOVERY_ONE_WINNER
- Exactly one recovery CAS may consume authorization and become G+1 owner.

WF-A4 FALLBACK_PRESERVED
- Previously verified recurring fallback remains enabled until recovery owner has exact STATE_OK on replacement prearm.

WF-A5 BOUNDED_RECOVERY
- Measure loss-marker -> recovery ownership commit -> verified owner rearm -> successor WORK_OK with GitHub server timestamps.
- Compare against directly relevant A-arm recovery if available. Without comparator, functional recovery may PASS but latency non-regression stays UNKNOWN.

WF-A6 NO_STRANDING
- If neither legal recovery CAS nor existing recurring fallback can produce legal owner/work resumption, B fails.

## Sample classification
A B-arm normal sample is CLEAN only if WF-N0..N6 and WF-S1..S4 all PASS. Immediate WRITE_OK+STATE_OK without later target-surviving WAKE_OK/WORK_OK is PROVISIONAL. Intentional supersession is SUPERSEDED_NOT_CLEAN. Post-hoc attribution repair is ATTRIBUTION_INCOMPLETE_NOT_CLEAN.

B promotion requires >=3 CLEAN normal samples plus one adverse sample passing WF-A1..A6, materially lower scheduler write/overwrite ambiguity, and no material continuity regression. Missing A-arm latency comparator remains explicitly unresolved.
