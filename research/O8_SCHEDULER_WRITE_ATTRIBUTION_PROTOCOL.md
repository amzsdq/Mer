# O8 Scheduler Write Attribution Protocol

Status: ACTIVE DIAGNOSTIC INSTRUMENTATION
Purpose: identify which concurrent invocation changed the single canonical scheduler without changing the scheduler policy under test.

## Problem
Automation live metadata exposes current schedule state but not writer identity. Gen34 forensic evidence proved the canonical DTSTART moved from `15:01:31Z` to `14:51:15Z` within ~4.7 seconds while substantive ownership remained gen34. Without per-write durable attribution, representation defects and concurrent-writer overwrites cannot be cleanly separated.

## Instrumentation
For every scheduler mutation during the O8 incident investigation, persist two immutable records.

### 1. SCHEDULER_WRITE_INTENT — before mutation
Required fields:
- invocation_id
- observed substantive generation / role
- handoff epoch if any
- mutation_reason: WAKE_START_PREARM | PROMPT_TRANSITION | RECOVERY | EARLY_EXIT_FALLBACK
- intended absolute DTSTART UTC
- full VEVENT text/hash
- timing_mode and intended enabled state
- representation path (`FULL_VEVENT_ABSOLUTE` only for the active representation differential)
- GitHub commit SHA/timestamp of the intent record

### 2. SCHEDULER_WRITE_RESULT — immediately after live readback
Required fields:
- intent record path/SHA
- update acknowledgement classification
- live automation ID
- live DTSTART/RRULE/timing_mode/enabled
- exact intended-instant match boolean
- mismatch classification if false
- GitHub commit SHA/timestamp of result record

## Race classification
If a later live readback differs from a previously verified target, search immutable intents/results whose GitHub timestamps fall between the two verified observations.
- exactly one matching later intent -> attributable overwrite candidate;
- multiple intents -> concurrent-writer race directly observed;
- no intent -> instrumentation gap or external/uninstrumented mutation; do not guess writer.

## Experimental discipline
This protocol is diagnostic instrumentation, not a change to the primary scheduler policy. During the first differential sample keep writer policy unchanged and change only representation to explicit full VEVENT. Only after that sample may `H-O8-SCHEDULER-WRITER-FENCE` become the primary variable.

## Success criterion
A future scheduler mismatch/overwrite must be attributable to a durable writer intent/result or explicitly classified `UNATTRIBUTED_INSTRUMENTATION_GAP`; never infer writer identity from timing alone.
