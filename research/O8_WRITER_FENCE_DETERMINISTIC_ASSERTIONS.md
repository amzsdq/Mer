# O8 Writer-Fence Deterministic Assertions

Status: ACTIVE TEST ORACLE
Hypothesis: `H-O8-SCHEDULER-WRITER-FENCE`

These assertions make the B-arm decision falsifiable without relying on prose interpretation.

## Normal handoff assertions
Given source generation G with one OPEN epoch targeting G+1:

WF-N1 SHADOW_WRITE_ZERO
- Before a contender wins fresh-SHA CAS, scheduler-write intent count for that contender MUST equal 0.

WF-N2 ONE_CAS_WINNER
- Across contenders bound to the same epoch, successful ownership transition count MUST equal 1.
- All stale-SHA/losing contenders remain non-authoritative.

WF-N3 POST_CAS_WRITE_ONE
- The CAS winner may issue exactly one normal scheduler-write intent/result pair after ownership acquisition.
- Generation/role in the intent must match the newly committed owner.

WF-N4 EXACT_STATE
- The post-CAS owner result must match canonical ID, enabled=true, exact_schedule, RRULE:FREQ=HOURLY, and exact intended absolute DTSTART.

WF-N5 WORK_RESUMES
- New owner must produce durable genuine-work progress after CAS/prearm; scheduler success alone is insufficient.

WF-N6 NO_DUPLICATE_AUTHORITY
- No authoritative shared-state mutation may be accepted from source generation G after the G+1 CAS commit.

## Stability assertions
WF-S1 NO_UNATTRIBUTED_OVERWRITE
- Between exact STATE_OK and the intended target, any changed live DTSTART must have a matching durable writer intent/result or be classified `UNATTRIBUTED_INSTRUMENTATION_GAP`; an unexplained overwrite fails clean-sample qualification.

WF-S2 WAKE_DISTINCT_FROM_STATE
- WRITE_OK+STATE_OK never implies WAKE_OK. WAKE_OK requires an actual later automation dispatch attributable to the intended recurring state or a documented provider-equivalent recurrence.

WF-S3 WORK_DISTINCT_FROM_WAKE
- WAKE_OK never implies WORK_OK. WORK_OK requires durable substantive progress in the resumed invocation.

## Owner-loss adverse assertions
WF-A1 EXPLICIT_SINGLE_USE_AUTH
- Recovery takeover requires an explicit durable single-use authorization scoped to exact source generation/owner/target generation. Elapsed time alone is insufficient.

WF-A2 SHADOW_STILL_NO_WRITE
- Even under owner loss, contender scheduler-write count remains 0 until legal recovery CAS succeeds.

WF-A3 RECOVERY_ONE_WINNER
- Exactly one recovery CAS may consume the authorization and become G+1 owner.

WF-A4 FALLBACK_PRESERVED
- The previously verified recurring fallback must remain enabled until the recovery owner has exact STATE_OK on its own replacement prearm.

WF-A5 BOUNDED_RECOVERY
- Measure owner-last-progress -> successor WORK_OK and compare with A-arm relevant recovery evidence. Do not declare non-regression without an explicit comparator or justified bound.

WF-A6 NO_STRANDING
- If neither legal recovery CAS nor existing recurring fallback can produce a legal owner/work resumption, B fails.

## Sample classification
A B-arm sample is CLEAN only if WF-N1..N6 and WF-S1..S3 all PASS. Immediate WRITE_OK+STATE_OK without later WAKE_OK/WORK_OK is PROVISIONAL, not CLEAN.

B may be promoted only after >=3 CLEAN normal samples plus one adverse sample passing WF-A1..A6, with scheduler write count/overwrite ambiguity materially improved and no material continuity regression.
