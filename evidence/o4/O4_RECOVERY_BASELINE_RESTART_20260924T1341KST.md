# O4 Recovery and Baseline Restart Evidence

Invocation: MER-O4-RECOVERY-20260924T1336KST
Stage: O4_WAKE_PREARM_OFFSET_OPTIMIZATION
Hypothesis: H-O4-ADAPTIVE-PREARM
Status: CONTINUE

## Fault removed
- spec/execution.json carried a stale duplicate generation value while control/ownership.json was already the declared ownership authority.
- status/program.json and spec/execution.json also pointed at the already-consumed gen9->gen10 handoff.
- The stale generation field was removed. control/ownership.json is now the sole durable generation authority.
- User-authorized single-use recovery advanced stale owner generation 10 -> 11.

## Normal continuation restored
- Current generation: 11
- Current owner: MER-O4-RECOVERY-20260924T1336KST
- OPEN epoch: O4_HANDOFF_EPOCH_GEN11_A840_001
- Target generation: 12
- Active sample: O4_FIXED_WINDOW_A_840S
- Baseline prearm: 840 seconds
- Candidate B after 3 clean A samples: 720 seconds
- Exactly one primary variable changes: WAKE_START_PREARM_OFFSET_SEC.

## Scheduler verification
Canonical automation: 6ab1fbfdaeb88191ac7257f0a2d607bd
enabled: true
timing_mode: exact_schedule
verified schedule:
BEGIN:VEVENT
DTSTART;TZID=Asia/Seoul:20260924T135513
RRULE:FREQ=HOURLY
END:VEVENT

The first recovery scheduler write returned a timezone-ambiguous DTSTART and was not accepted as secured continuation. One immediate corrective recovery write added explicit TZID=Asia/Seoul and the subsequent live metadata read matched the intended schedule.

WRITE_OK=YES
STATE_OK=YES
WAKE_OK=PENDING_FUTURE_EXECUTION
WORK_OK=YES

## Recovery commits
- START marker: 0e7458b0ff0dfba10539a32b5913f600bde0345b
- ownership recovery gen10->11: 432b520bd533c3606685a4e603f3e3684f591f37
- execution authority reconciliation: 7dd22728d7dc51794d2363f1e6caaf67fb0c9103
- program continuation reconciliation: 12eba9c94a3e7e61f62bf6cd1edcfab08d3ff269
- normal gen11->12 epoch open: 35096eb3ced679451e1daebfe2219ca6cae16558
- prompt manifest / live schedule sync: 2b19dcf2c8181811edbca1cb49f795da6342ef36

## Next
At the next actual wake, the successor must perform its one wake-start prearm, reconstruct current state, bind READY evidence to O4_HANDOFF_EPOCH_GEN11_A840_001 / generation 11, and attempt exactly one fresh-SHA CAS to generation 12. If successful, record clean A sample 1/3 and immediately continue under generation 12.
