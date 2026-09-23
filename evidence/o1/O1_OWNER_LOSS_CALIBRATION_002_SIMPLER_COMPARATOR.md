# O1 OWNER LOSS CALIBRATION 002 — SIMPLER COMPARATOR

Status: IMMUTABLE_SHADOW_ANALYSIS

## Comparator question
Can owner-loss liveness be recovered without a time-based lease?

## Candidate simpler mechanisms
1. Owner-mediated handoff only: already falsified for owner disappearance; liveness hole remains.
2. Any SHADOW may CAS takeover immediately when it sees old owner: unsafe because 'old' cannot be distinguished from slow/alive without a durable failure detector; can create premature takeover.
3. Fixed generation baton with predeclared successor allowed to CAS immediately after READY: reduces contender ambiguity but still cannot safely distinguish predecessor alive vs lost unless transfer authority is explicitly delegated before loss.
4. Predelegated transfer token written by owner before substantive work: potentially simpler than a renewable lease for planned handoff, but does not recover loss occurring before token publication and changes the primary transfer protocol rather than only owner-loss recovery.

## Comparator result
For unplanned owner disappearance, some failure-detection signal is required. Under current primitives, bounded durable non-renewal is the minimal viable signal found so far. The lease candidate remains justified, but only for owner-loss recovery; normal healthy transfer should stay owner-mediated and avoid lease waiting.

## Test implication
Do not replace normal handoff with leases. Test lease expiry strictly as a fallback path. This keeps the common path simpler and confines timing uncertainty to recovery.
