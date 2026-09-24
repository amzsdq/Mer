# O8 Scheduler Writer Fence Test Plan

Status: PREPARED, NOT ACTIVE
Depends on: attributed full-VEVENT representation sample later WAKE_OK/WORK_OK result.
Primary hypothesis: `research/H-O8-SCHEDULER-WRITER-FENCE.md`.

## Why this is separate
Gen34 proved two facts: full-VEVENT absolute representation can achieve exact WRITE_OK+STATE_OK, and the canonical scheduler can be overwritten by another writer while substantive ownership remains unchanged. These are separate variables. Writer fencing must not be activated until the representation sample's later wake/work result is known.

## A/B variable
A — ALL_WAKES_PREARM (current baseline): every invocation may perform one scheduler prearm regardless of substantive ownership.
B — OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY: a SHADOW may read/prepare/READY but does not mutate the scheduler; only current ACTIVE_OWNER or a successor after successful generation CAS writes the canonical scheduler.

Hold constant:
- same canonical automation ID;
- prompt version except the minimal writer-authority clause needed for B;
- full VEVENT absolute DTSTART representation;
- RRULE:FREQ=HOURLY, exact_schedule, enabled=true;
- offset=840s;
- generation-CAS substantive authority;
- role-relative stop semantics;
- work package/admission policy;
- write-intent/result attribution.

## Metrics per sample
- number of scheduler write intents/results;
- exact intended/readback match rate;
- later WAKE_OK and WORK_OK;
- target overwrite count and direction/magnitude;
- duplicate authoritative side effects;
- READY->CAS latency;
- actual idle/handoff gap;
- recovery latency after owner loss;
- stranded-continuation events.

## Normal samples
Minimum three clean samples per arm unless an arm fails a hard continuity/authority gate earlier. A clean sample requires attributed scheduler writes, actual later wake/work, exactly one substantive owner, and no unexplained target overwrite.

## Adverse owner-loss sample — mandatory before B promotion
Purpose: test the strongest counterargument to owner-only scheduler writes.
1. Current owner has a previously verified future recurring fallback.
2. A successor wakes/exists as SHADOW but has not yet won CAS.
3. Simulate/observe predecessor loss before transfer without allowing SHADOW scheduler mutation.
4. Require one of:
   - successor can legally acquire generation via existing recovery rule and then rearm as new owner; or
   - existing recurring fallback wakes a legal recovery path within bounded latency.
5. FAIL B if continuation becomes stranded or recovery latency materially worsens versus A without compensating reliability benefit.

## Promotion decision
KEEP B only if:
- continuity/recovery is no worse under tested conditions;
- scheduler write count decreases;
- overwrite ambiguity materially decreases;
- no duplicate substantive authority appears;
- owner-loss adverse sample passes.

Otherwise KEEP A or REVISE to a durable scheduler-write claim/epoch that allows exactly one scheduler writer per generation while preserving explicit recovery takeover.

## Evidence discipline
Every mutation uses `research/O8_SCHEDULER_WRITE_ATTRIBUTION_PROTOCOL.md`. A changed live DTSTART without a matching durable intent is `UNATTRIBUTED_INSTRUMENTATION_GAP`, not proof of a specific writer.
