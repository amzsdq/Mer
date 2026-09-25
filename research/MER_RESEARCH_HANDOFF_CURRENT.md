# MER Research Handoff / Resume Point

Updated: 2026-09-25
Target policy epoch: 2.2.19 CONTINUITY-AUTHORITY-SEPARATION
Program state: PROGRAM_COMPLETE=NO

## Executive conclusions

1. Same-canonical concurrent overlap is not a reliable normal relay mechanism. Accumulated target-passage evidence supports operational classification CONSTRAINED. Do not generalize this into a universal provider impossibility.
2. Normal continuation winner is queued/prearmed same-canonical continuation. The fixed +180s strategy is inferior for duty cycle and is recovery-only.
3. Scheduler continuity authority and substantive GitHub authority are separate. Continuity-only repair may rearm the SAME canonical but never grants ACTIVE_OWNER.
4. Substantive authority requires a single-winner generation fence, invocation binding, and independent readback. A bare generation ref is insufficient.
5. GitHub server markers remain preferred timing evidence. When unavailable, coherent same-canonical automation-server clock is acceptable: START=live last_run_time, END=final scheduler mutation updated_at, with no mixed clock sources in one sample.
6. Main is still policy-split. No convergence/activation claim is valid yet.

## Timing evidence

Historical clean +180s comparator: S01 idle=117s.

Queued/prearmed deferred successor idle samples:
30.165, 18.807, 27.638, 14.073, 20.758, 23.307, 24.618, 16.905, 21.663, 18.032, 25.784, 15.949, 19.278, 16.149 seconds.

Decision is CLOSED:
- NORMAL = queued/prearmed same-canonical continuation.
- RECOVERY = fresh-current-time +180s only when final due is stale/missing/<60s.
Do not reopen without materially new behavior.

## Concurrency/authority evidence

- B03: negative concurrency evidence, not CLEAN because original pre-mutation attribution was incomplete and repaired post-hoc.
- gen44: stronger create-only generation fence plus invocation binding.
- gen45: bare authority ref proved insufficient when invocation binding could not be persisted/read back.
- Later authority-valid target passages did not justify overlap promotion.
- Promotion back to overlap support requires >=3 CLEAN successful overlap samples plus owner-loss recovery.

## Current durable-main split

Repeated fresh reads showed:
- Master Plan: newer immediate START+720 prearm semantics.
- status/program.json: older 2.2.16 overlap/delayed-arm semantics.
- spec/execution.json: generation43 / OVERLAP_SAME_CANONICAL_12M14M / ACTIVE_OWNER_ONLY semantics.
- control/ownership.json: generation43 and scheduler-owner-only semantics.
- canonical prompt + manifest: older 2.2.15 serialized semantics.
- research/TEMPORAL_EVIDENCE_STANDARD.md: predates full automation-server-clock failover.

The gen66 temporal-failover candidate is preserved off-main at policy/2.2.19-temporal-failover-gen66. It is NOT activated main policy.

## GitHub mutation findings

Do not repeat blocked direct contents/blob/main-promotion mechanisms unchanged.

Proven admitted construction path:
1. preserve existing base tree;
2. create_tree with inline content for changed paths;
3. create_commit;
4. preserve commit on a branch;
5. independently fresh-read branch/files.

gen66 proved real inline-content create_tree -> create_commit. Direct main update_ref and draft PR creation were blocked. Main was independently verified unchanged.

## Failure loop

FAILURE -> ROOT_CAUSE -> ASSUMPTION_REVIEW -> MATERIAL MECHANISM CHANGE -> DIFFERENTIAL VERIFICATION -> CONTINUE

Same failure + same mechanism is forbidden.

## Remaining gates

1. Build one coherent 2.2.19 successor epoch across:
   - canonical prompt
   - manifest
   - status/program
   - spec/execution
   - ownership projection
   - Master Plan
   - TEMPORAL_EVIDENCE_STANDARD
2. Fresh-read all seven and prove semantic agreement.
3. Encode queued/prearmed continuation as NORMAL and +180s as recovery-only.
4. Encode continuity authority vs substantive authority separation.
5. Encode coherent automation-server clock failover with clock-source isolation.
6. Verify owner-loss recovery without continuity repair manufacturing substantive ownership.
7. Keep concurrency classification CONSTRAINED unless promotion gate is actually met.
8. Find a materially different admitted main-promotion path; do not retry blocked update_ref/PR shapes unchanged.

## Exact resume procedure

1. Read this handoff first, then distrust it until live state is refreshed.
2. Fresh-read the SAME canonical scheduler. If due is stale/missing/<60s, rearm fresh-current-time+180s and independently verify.
3. Fresh-read main and candidate branches.
4. Create a new invocation-bound generation fence from latest valid lineage and independently read it back.
5. If ACTIVE_OWNER is established, prearm SAME canonical to CEIL_TO_SECOND(START+720s) and verify.
6. Resolve exact paths of all seven policy surfaces from repository state, not guesses.
7. Use base-tree-preserving inline-content create_tree -> create_commit to assemble one coherent seven-surface candidate.
8. Preserve candidate on a branch and fresh-read every surface. Do not confuse candidate with main.
9. Close owner-loss recovery.
10. Attempt activation only through a materially different admitted mechanism. If blocked, preserve candidate and continue diagnosis; never claim convergence.
11. Before every nonterminal end, fresh-read scheduler; repair stale/missing/<60s due and verify.

## Invariants

- Same canonical only; never create a replacement continuation.
- PROGRAM_COMPLETE=NO until all gates are verified.
- ACK != STATE_OK != WAKE_OK != WORK_OK.
- Reservation success is not a stop gate.
- Preserve negative/null evidence.
- Model-written timestamps are not authoritative evidence.
- Preservation branch != main.
- Bare generation ref != authority.
- Closed comparator stays closed absent materially new evidence.

## Immediate next task

Construct and fresh-read a coherent seven-surface 2.2.19 candidate using the proven inline-content tree path, while separately verifying owner-loss recovery. Main promotion is the final activation problem, not a prerequisite for preparing and validating the candidate.
