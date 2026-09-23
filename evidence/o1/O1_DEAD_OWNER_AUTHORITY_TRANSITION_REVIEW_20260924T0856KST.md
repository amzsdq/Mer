# O1 dead-owner authority-transition review

## Finding
The current experiment is structurally unable to satisfy its own next_action.

- spec/execution.json requires >=5 GENUINE_OWNER_RENEWAL_GAP samples before deriving expiry.
- control/ownership.json still names MER-20260924T060808+0900-O1BOOT001 as generation-2 OWNER with owner_renewal_seq=1.
- That invocation is no longer the executing invocation. Current and later wakes are SHADOW under the deployed kernel.
- A SHADOW cannot create genuine OWNER renewals and cannot revise the authoritative experiment/ownership record.

Therefore additional SHADOW wakes cannot make progress toward the declared primary variable. Continuing to collect scheduler wakes as if they could satisfy owner-renewal calibration would be busywork and violates the research rule against unchanged retries after diagnosis.

## Important correction to the handoff-epoch candidate
OWNER_OPENED_HANDOFF_EPOCH is useful only for graceful handoff while a live OWNER exists. It cannot repair the already-dead generation-2 owner, because only that dead OWNER would be allowed to OPEN the epoch. Therefore it is not an escape mechanism for the current state.

## Minimal legal recovery primitive needed
The experiment needs one explicit abnormal-recovery authority transition that is distinct from normal handoff. Candidate semantics:

RECOVERY_EPOCH_OPENED_BY_PROGRAM_AUTHORITY -> READY_SHADOW_CANDIDATE -> fresh-read CAS generation G to G+1 -> old generation fenced.

The recovery epoch must be explicitly authorized by durable program/execution state, not inferred from elapsed time, scheduler wake, or contender readiness. It should be single-use and scoped to the exact stuck generation. This avoids inventing an uncalibrated expiry while retaining optimistic concurrency and generation fencing.

This is not yet authorized by current execution state, whose bootstrap_exception_authorized=false and no_ad_hoc_self_promotion=true. Accordingly this invocation MUST NOT perform the takeover.

## Research decision
REVISE the next experiment design: separate three mechanisms.
1. Graceful handoff: OWNER-opened handoff epoch + first valid READY contender CAS.
2. Abnormal bootstrap/dead-owner escape: program-authorized single-use recovery epoch scoped to stuck generation.
3. Autonomous future owner-loss detection: lease/failure-detector calibration, tested only after a live OWNER lineage exists.

This removes the circular dependency in which a dead OWNER must generate renewal data before recovery from its own death can be tested.

## Required authoritative revision
An authorized OWNER/program-authority invocation should revise status/program.json and spec/execution.json so the immediate primary variable is SINGLE_USE_PROGRAM_AUTHORIZED_RECOVERY_EPOCH, with one recovery canary. On success, the new live OWNER can run graceful handoff tests and collect genuine renewal/liveness data. Autonomous expiry remains unpromoted until calibrated.
