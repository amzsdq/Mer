# O8 Scheduler Write Attribution Protocol

Status: ACTIVE DIAGNOSTIC INSTRUMENTATION
Purpose: identify which invocation changed the single canonical scheduler and distinguish legal owner writes from races or uninstrumented mutation.

## Problem
Automation live metadata exposes current schedule state but not writer identity. Gen34 forensic evidence proved canonical DTSTART changed while substantive ownership remained gen34. Without per-write durable attribution, representation defects, legal recovery, and concurrent-writer overwrites cannot be separated.

## Instrumentation
For every scheduler mutation during O8, persist two immutable records. Never repair an incomplete immutable intent by rewriting it; use an immutable addendum and downgrade attribution completeness.

### 1. SCHEDULER_WRITE_INTENT — before mutation
Required fields:
- invocation_id;
- observed substantive generation and role;
- handoff epoch if any;
- mutation_reason: OWNER_NORMAL_PREARM | POST_CAS_NEW_OWNER_NORMAL_PREARM | PROMPT_TRANSITION | RECOVERY | EARLY_EXIT_FALLBACK;
- intended absolute DTSTART UTC;
- full VEVENT text and, when available, deterministic hash;
- timing_mode and intended enabled state;
- representation_path=`FULL_VEVENT_ABSOLUTE` for the active candidate;
- durable intent path plus GitHub commit SHA/timestamp once created.

For owner-fenced B samples, the intent must additionally state `shadow_scheduler_write_count` and the ownership/CAS evidence that made this invocation a legal writer.

### 2. SCHEDULER_WRITE_RESULT — immediately after live readback
Required fields:
- intent record path/SHA;
- update acknowledgement classification;
- live automation ID;
- live DTSTART/RRULE/timing_mode/enabled;
- exact intended-instant match boolean;
- mismatch classification if false;
- GitHub result commit SHA/timestamp once persisted.

## Race classification
If a later live readback differs from a verified target, inspect immutable intents/results between observations.
- exactly one matching later legal intent -> attributable overwrite candidate;
- multiple intents -> concurrent-writer surface observed; determine legal/illegal writer status from generation/role evidence;
- no intent -> `UNATTRIBUTED_INSTRUMENTATION_GAP`; do not guess writer.

## Completeness classification
- `ATTRIBUTION_COMPLETE`: all required pre-mutation intent fields existed before scheduler mutation and result was persisted after exact readback.
- `ATTRIBUTION_ADDENDUM_REQUIRED`: mutation behavior may still be valid, but one or more required intent fields were reconstructed only after mutation. This cannot silently become ATTRIBUTION_COMPLETE.
- `UNATTRIBUTED_INSTRUMENTATION_GAP`: no matching durable intent.

A CLEAN normal sample should prefer ATTRIBUTION_COMPLETE. If an addendum is required, the sample may remain useful mechanism evidence but must not be counted CLEAN unless the deterministic oracle explicitly allows that limitation and the promotion decision explains why it cannot affect writer identification.

## Experimental discipline
Instrumentation itself is not the primary scheduler variable. Writer-fence B holds full-VEVENT representation fixed and changes only legal scheduler-writer authority.

## Success criterion
Every future scheduler mismatch/overwrite is either attributable to durable writer evidence or explicitly classified as an instrumentation gap. Writer identity is never inferred from timing alone.
