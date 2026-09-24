# O6 deterministic orphan-recovery fixture suite

Hypothesis: H-O6-STRANDED-OWNER-RECOVERY
Primary variable: RECOVERY_MODE

Results:
- T1 stranded owner exact reproduction: PASS. Eligible deterministic recovery advances exactly one generation, produces no duplicate authoritative side effect, keeps scheduler enabled, and opens a legal next epoch.
- T2 stale shadow after authority advanced: PASS. Fresh-read generation mismatch rejects recovery before CAS; zero authoritative mutation.
- T3 OPEN epoch with late/not-ready successor: PASS. Existing legal handoff path rejects recovery; OPEN epoch preserved.
- T4 two contenders on same source SHA: PASS. One fresh-SHA CAS wins; loser observes conflict, re-reads advanced generation, and fails closed as SHADOW; one generation increment.
- T5 unreconstructable authority: PASS. No recovery mutation; scheduler remains enabled.

Decision: KEEP the exact-generation orphan-recovery admission model as an O6 recovery primitive, but DO NOT declare Stage O6 complete. These are isolated deterministic state-machine fixtures. They validate the proposed admission/fencing semantics without corrupting production authority, but the Master Plan additionally requires adverse recovery coverage for missed intended wake, predecessor termination, stale state, duplicate/competing actor, prompt mismatch, missing bootstrap, and accepted scheduler write followed by absent wake. T1-T5 cover predecessor termination/stranded owner, stale state, competing actor, and malformed authority only. Prompt mismatch, missing bootstrap, and scheduler WRITE_OK-without-WAKE_OK still require discriminating tests.

Promotion status: KEEP_PRIMITIVE / STAGE_GATE_OPEN.

Next research question: whether the existing prompt-sync/bootstrap/scheduler fallback mechanisms recover correctly under three remaining O6 fault classes without introducing scheduler disablement, duplicate authority, or false WRITE_OK=>WAKE_OK inference.
