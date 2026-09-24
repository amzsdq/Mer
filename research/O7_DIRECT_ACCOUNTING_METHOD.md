# O7 Direct Useful/Control/Idle Accounting Method

Status: ACTIVE TEST METHOD

Purpose: close O7 utilization evidence without treating total invocation elapsed time as useful work.

## Observable interval classes

1. USEFUL — interval bounded by durable GitHub markers around a substantive plan-defined unit whose output changes research evidence, decision state, test result, or validated artifact.
2. CONTROL — interval bounded by durable markers around bootstrap/prompt-sync, scheduler mutation/readback, authority CAS/handoff, and close bookkeeping when these operations do not themselves answer the research question.
3. IDLE — directly observable gap between predecessor END and successor START, or between a verified scheduler target and actual successor START when no invocation was executing. Scheduler lateness is reported separately and is not automatically identical to idle if overlap exists.
4. UNKNOWN — any interval that cannot be assigned from direct external timestamps. UNKNOWN is never silently allocated to USEFUL.

## Clock

GitHub server timestamps on durable marker commits are the authority for USEFUL/CONTROL interval boundaries. Scheduler target/readback timestamps may measure scheduler lead/lateness but do not replace GitHub work clocks.

## Sample record

For each final-kernel sample persist:
- invocation_id, generation, predecessor_generation
- START_MARKER path/timestamp
- END_MARKER path/timestamp when legal close occurs
- zero or more USEFUL_UNIT_START/END marker pairs with unit_id and durable consequence
- zero or more CONTROL_UNIT_START/END marker pairs with control_type
- verified scheduler DTSTART and actual successor START
- predecessor END -> successor START gap when both exist
- UNKNOWN_SEC = WORKED - directly classified USEFUL_SEC - directly classified CONTROL_SEC, only when WORKED exists and intervals do not overlap

## Metrics

Within directly observed non-overlapping intervals:
- useful_fraction = USEFUL_SEC / (USEFUL_SEC + CONTROL_SEC + IDLE_SEC)
- control_fraction = CONTROL_SEC / denominator
- idle_fraction = IDLE_SEC / denominator
- unknown_fraction is reported separately and prevents overclaiming coverage.

No metric is promoted when marker coverage is too sparse to discriminate candidate mechanisms. The purpose is mechanism comparison, not maximizing a self-reported number.

## Admission rule

Marker creation must remain bounded and must not dominate the workload. Instrument only meaningful unit boundaries. If instrumentation cost materially changes the mechanism under test, record that as measurement interference and use a coarser bounded package.
