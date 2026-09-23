# F4 workload

Primary variable versus F3: desired and observed generations match; semantic phase consistency is intentionally false.

Procedure:
1. Read the execution spec referenced by control/active.json.
2. Read status/current.json and compare generation values.
3. After generation equality passes, compare desired_phase with observed_phase.
4. If the phase values conflict for the same generation, classify STATE_NOT_RECONCILED_SEMANTIC_CONFLICT.
5. Persist compact evidence and preserve the existing continuation method.

Expected: matching generations alone are insufficient; contradictory semantics prevent a reconciliation claim.
