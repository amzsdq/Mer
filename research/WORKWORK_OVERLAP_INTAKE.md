# workwork Overlap Evidence Intake

Status: PRIOR_EVIDENCE_ACCEPTED_WITH_LIMITS
Date: 2026-09-23

## What is directly proven

Source: amzsdq/workwork/overlap/OVERLAP-15M-WAKE12M-01/result.json

Observed:
- primary_start: 2026-09-23T01:17:18+09:00
- scheduled_overlap_wake: 01:29:18
- successor/observer_start: 01:29:22
- wake jitter: +4s
- primary_end: 01:32:40
- observed simultaneous execution: 198s
- classification: CONCURRENT

Interpretation:
The same recurring automation can produce a successor invocation while the predecessor invocation remains alive. The successor did not preempt or serialize behind the predecessor in this probe.

This is strong prior evidence that overlap is technically feasible.

## What is NOT proven

A concurrent wake is not the same as a clean handoff.

workwork/SUCCESSOR_HANDOFF_RESEARCH.md explicitly requires:
- exactly one ACTIVE_OWNER;
- SHADOW successor preparation without authoritative side effects;
- READY;
- fenced generation/CAS transfer;
- no predecessor authoritative work after transfer;
- successor acceptance and immediate work;
- no scheduler-writer collision;
- no checkpoint loss.

Observed later workwork handoff records repeatedly show:
- READY successors existed;
- duplicate_authoritative_work_count remained 0 in the inspected recovery case;
- scheduler_writer_conflicts remained 0 in the inspected recovery case;
BUT
- transfers were classified FENCED_RECOVERY_NOT_CLEAN_NORMAL_HANDOFF;
- clean_handoff=false;
- some READY-to-accept delays were tens of minutes.

Therefore:
OVERLAP_CONCURRENCY = PROVEN_IN_WORKWORK
CLEAN_NORMAL_HANDOFF = NOT_YET_PROVEN
HIGHEST_LONG_RUN_UTILIZATION = NOT_YET_EMPIRICALLY_PROVEN

## Why use overlap as Mer baseline anyway

Mer's primary objective is continuity first, then maximum long-run useful-work utilization.

A non-overlap policy has an unavoidable positive handoff/boot gap unless scheduler jitter accidentally overlaps it.
A controlled overlap policy can move successor bootstrap/preparation inside predecessor productive time, so its attainable utilization ceiling is higher.

This establishes overlap as the high-ceiling baseline candidate, NOT as a validated final policy.

## External prior

Kubernetes Lease/leader-election patterns support the coordination shape:
- multiple instances may exist;
- one holderIdentity owns authority;
- optimistic concurrency/versioning selects one holder;
- non-holders remain candidates/followers.

Transferable to Mer:
- concurrent liveness does not imply concurrent authority;
- one authoritative owner/writer must be fenced by durable generation/version state.

Not transferable:
- Kubernetes lease timing/defaults and safety guarantees do not automatically apply to ChatGPT Automations.

## Mer baseline hypothesis

H-OVERLAP-BASE-1:
A controlled predecessor + SHADOW successor overlap, with exactly one authoritative owner and one scheduler writer, can achieve lower handoff idle than the best non-overlap policy without duplicate authoritative side effects or continuation loss.

Initial prior-informed parameters:
- owner nominal useful-work horizon: 600s (Mer experimental starting point)
- successor lead: 180s
- successor wake target: owner_start + 420s
- overlap target: about 180s if owner reaches nominal horizon
- same recurring automation
- active owner schedules once; shadow does not schedule
- authority transfer uses generation + optimistic-concurrency/CAS
- immutable per-invocation evidence

The 180s lead comes from workwork's proven ~3m technical overlap and handoff research starting point. It is an experimental parameter, not a promoted constant.

## Primary falsification conditions

Reject/revise the baseline if repeated Mer samples show any of:
- successor does not reliably start while predecessor remains alive;
- duplicate authoritative side effects;
- scheduler-writer collisions;
- ownership ambiguity;
- handoff/recovery overhead erases the utilization advantage;
- continuity materially worse than the non-overlap comparator.

## Comparator

Retain existing Mer immediate-prearm non-overlap observations as comparator evidence.
Do not discard them; they are not the primary baseline anymore.
