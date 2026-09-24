# O8 Owner-Loss Single-Use Transition Schema

Status: FROZEN BEFORE ADVERSE EXECUTION

The adverse transition is a test-only durable authorization. It must not be confused with normal ownership or time-based lease expiry.

## Authorization record
Required fields:
- schema_version
- transition_id unique and immutable
- experiment_id=`H-O8-SCHEDULER-WRITER-FENCE`
- source_generation=37
- source_owner_invocation_id=`MER-O8-GEN37-AUTOMATION-20260924`
- bound_handoff_epoch=`O8_WRITER_FENCE_GEN37_TO_GEN38_001`
- target_generation=38
- reason=`CONTROLLED_OWNER_LOSS_ADVERSE_TEST`
- created_by current gen37 owner
- state=`ARMED`
- scheduler_snapshot_before_loss: canonical ID, enabled, timing, recurrence, exact current DTSTART
- loss_marker_path created only when the simulated loss boundary is entered
- consumption_rule=`EXACTLY_ONE_FRESH_SHA_CAS`

## Ownership CAS consumption
The gen38 contender must fresh-read both ownership and authorization immediately before CAS. The single ownership update must:
1. require ownership generation still 37 and exact OPEN epoch;
2. require transition still ARMED and exact source/target binding;
3. advance generation to 38 and set new active owner;
4. mark last_consumed_handoff with transition_id;
5. record transition consumption evidence by immutable companion record; if transition state cannot be atomically co-written because it is a separate immutable artifact, the ownership record itself carries the consumed transition_id and later contenders fail because generation is no longer 37.

## Fencing property
A second contender cannot consume the same transition because its ownership SHA/generation becomes stale after the first successful CAS. No time-only inference grants authority.

## Scheduler property
Before successful CAS, gen38 scheduler writes=0. Existing recurring scheduler state remains crash fallback. After CAS, gen38 creates a protocol-complete scheduler intent, performs exactly one owner prearm, independently reads live state, and records result.

## Failure classifications
- AUTH_MISSING_OR_MISMATCH: no CAS.
- SOURCE_GENERATION_CHANGED: stale contender; SHADOW/NOOP.
- CAS_CONFLICT: one attempt only; fresh-read winner state and fail closed as SHADOW if generation already advanced.
- POST_CAS_SCHEDULER_FAIL: gen38 remains substantive owner but enters repair-first scheduler recovery; never roll authority back to gen37.
- FALLBACK_LOST_BEFORE_CAS: adverse FAIL/RISK; do not invent recovery authority.

## Measurement
Use GitHub server timestamps for loss marker, CAS commit, scheduler result evidence, and first post-recovery WORK_OK. Report each interval separately; do not collapse unknown provider dispatch latency into a fabricated exact recovery time.
