# O8 Owner-Loss Adverse Protocol

Status: FROZEN FOR NEXT SAMPLE
Hypothesis: H-O8-SCHEDULER-WRITER-FENCE
Primary variable remains scheduler writer eligibility; full-VEVENT representation, 840s offset, recurrence, exact_schedule, enabled=true, generation-CAS authority, and work admission stay fixed.

## Purpose
Test the strongest failure mode introduced by owner-only scheduler writes: the active owner disappears before ordinary handoff while a SHADOW is prohibited from touching the scheduler.

## Safety constraint
Do not corrupt or delete authoritative state and do not disable the canonical automation. The test simulates owner loss only by durably declaring the current test owner unavailable for further authoritative work under a single-use adverse transition. The previously verified recurring schedule remains intact as crash insurance.

## Single-use transition
The current gen37 owner may create exactly one immutable authorization record naming:
- source generation 37 and owner invocation;
- open epoch `O8_WRITER_FENCE_GEN37_TO_GEN38_001`;
- expected recovery successor generation 38;
- reason `CONTROLLED_OWNER_LOSS_ADVERSE_TEST`;
- consumed=false initially;
- scheduler state observed immediately before simulated loss.

After the loss marker, gen37 performs no further authoritative state or scheduler writes. A gen38 contender remains SHADOW and performs zero scheduler writes until it has READY evidence bound to the exact epoch and wins one fresh-SHA CAS that atomically consumes the single-use transition. Only then may gen38 rearm the scheduler as ACTIVE_OWNER.

## Required evidence
1. pre-loss live scheduler state: same canonical, enabled, exact_schedule, RRULE present, future DTSTART;
2. loss marker GitHub server timestamp;
3. gen38 START and READY evidence with shadow scheduler writes=0;
4. one fresh-SHA CAS consuming the exact transition and advancing generation 37->38;
5. gen38 owner scheduler intent/result and exact live readback;
6. first post-recovery WORK_OK;
7. recovery latency measured from loss-marker server timestamp to gen38 ownership commit and to verified owner rearm;
8. duplicate substantive authority count=0 and stranded-continuation=false.

## PASS
PASS only if generation 38 legally acquires authority without any pre-CAS SHADOW scheduler write, the same canonical remains enabled/recurring, post-CAS owner rearm is WRITE_OK+STATE_OK, useful work resumes, and no duplicate authority or stranded continuation occurs.

## FAIL / REVISE
FAIL B if recovery requires a forbidden SHADOW scheduler write, the canonical becomes disabled/nonrecurring, no legal generation takeover exists, or continuation strands. REVISE if recovery works but latency is materially worse than the all-wakes baseline; candidate revision is a durable single scheduler-writer claim/epoch with explicit takeover rather than unrestricted all-wakes mutation.

## Nonclaim
Immediate WRITE_OK+STATE_OK is not a clean normal sample and does not prove future WAKE_OK. The adverse result does not replace the requirement for three clean later-wake normal samples.
