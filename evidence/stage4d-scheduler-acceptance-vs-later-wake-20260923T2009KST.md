# Stage 4D — scheduler acceptance vs later wake

Observed non-destructive condition, 2026-09-23 KST.

## Distinction
1. scheduler update accepted: the schedule write API returns success.
2. verified live state: the automation object/readback reflects the requested schedule and remains enabled.
3. later wake: a subsequent scheduled invocation actually occurs.

These are separate evidence classes. (1) and (2) cannot prove (3).

## Existing observation
During this research, schedule updates were accepted and live state reflected the requested DTSTART/RRULE, while the subsequent wake had to be evaluated only when it actually arrived. The present invocation itself is evidence that the prior accepted/live schedule did later wake on this path; conversely a hypothetical missing later wake cannot be detected at write time by the worker that is about to exit.

## Candidate discrimination
C_POINTER_ONLY contains scheduler survival/write rules but no independent liveness observer. If a schedule write is accepted and live state looks correct, then the worker can report WAKE_OK=PENDING. If the later wake never happens, C cannot self-detect that absence because no execution exists to perform the check. Recovery therefore requires an external/independent liveness observer or another wake source.

HYBRID vs POINTER_ONLY does not materially change this physical limitation when both rely on the same sole scheduler. Stable prompt rules improve correct rearm behavior, but they do not turn acceptance into proof of future execution.

## Result
PASS as a discriminating failure test: acceptance, live-state verification, and later wake must remain separate metrics. For the missing-later-wake condition, candidate-local detection/recovery = unavailable without an independent observer/wake source. Do not intentionally break the live scheduler to demonstrate this.
