# O8 Clean B Sample Template

Use for B04+ normal samples. A sample cannot count CLEAN unless every field below has durable evidence.

## Before CAS — SHADOW
- invocation_id
- START marker path/SHA/server timestamp
- source_generation / target_generation
- bound OPEN epoch
- scheduler_write_count_as_shadow = 0
- READY evidence path/SHA

## Ownership CAS
- pre-CAS ownership blob SHA
- one update attempt
- winning ownership commit SHA/server timestamp
- new generation / owner
- losing contender evidence if any

## Pre-mutation scheduler intent — must exist BEFORE mutation
- intent path/SHA/server timestamp
- generation / role=ACTIVE_OWNER
- consumed handoff epoch
- mutation_reason=POST_CAS_NEW_OWNER_NORMAL_PREARM
- shadow_scheduler_write_count=0
- ownership commit SHA
- reference UTC + offset=840s + exact intended DTSTART UTC
- complete VEVENT text
- timing_mode=exact_schedule
- intended_enabled=true
- representation_path=FULL_VEVENT_ABSOLUTE

## Mutation + immediate result
- update acknowledgement
- independent live readback
- same automation ID
- enabled=true
- exact_schedule
- RRULE:FREQ=HOURLY
- exact intended DTSTART
- result path/SHA/server timestamp

## Stability to dispatch
- no target mutation before qualifying dispatch, OR every mutation fully attributed and sample classification updated
- actual later last_run_time / successor START evidence
- WAKE_OK classification
- resumed WORK_OK durable evidence
- no duplicate authoritative side effect
- predecessor stale-write audit after CAS

## Classification
- CLEAN only if deterministic oracle WF-N0..N6 + WF-S1..S4 all PASS.
- PROVISIONAL if target has not yet produced qualifying later wake/work.
- SUPERSEDED_NOT_CLEAN if target intentionally changes before dispatch.
- ATTRIBUTION_INCOMPLETE_NOT_CLEAN if any required pre-mutation attribution was added only after the scheduler mutation.
- FAIL with exact failed assertion otherwise.
