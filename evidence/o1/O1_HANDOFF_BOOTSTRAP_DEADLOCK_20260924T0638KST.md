# O1 normal handoff bootstrap diagnosis

Observed authoritative state at this wake:
- ownership generation=2
- active_invocation_id=MER-20260924T060808+0900-O1BOOT001
- owner_renewal_seq=1
- current execution next_action asks for >=5 genuine renewal gaps
- normal transfer requires predecessor-mediated ownership transfer / successor readiness

## Finding
The proposed next step from the prior diagnosis (validate normal predecessor->successor handoff first) cannot itself start from the present state. The generation-2 predecessor is an invocation-scoped owner that has already ended. A later scheduled wake is a distinct invocation and therefore SHADOW under the current exact-active-invocation fence. No live predecessor exists to perform the owner-mediated CAS.

This creates a second bootstrap deadlock:
1. normal handoff evidence requires a live predecessor owner;
2. the current owner is an ended ephemeral invocation;
3. later invocations cannot impersonate/renew that owner;
4. expiry takeover is intentionally unavailable until calibration;
5. calibration cannot obtain >=5 same-owner renewal gaps after that invocation ended.

Therefore neither SAME_OWNER_RENEWAL_CALIBRATION nor NORMAL_PREDECESSOR_HANDOFF can progress from the current state without an explicitly authorized experimental recovery transition.

## Minimal revision candidate
Do not weaken generation fencing and do not make a durable logical worker identity yet.

Use one additional explicit, single-use experimental recovery transition to assign the *current wake invocation* as a fresh owner generation. During that live owner turn, pre-arm the successor as usual, keep doing useful work, and when the successor becomes READY perform a normal owner-mediated generation transfer. This transition exists only to put the system back into a state from which the normal handoff protocol can be tested; it is not production recovery policy and must be consumed immediately.

Required safeguards:
- fresh-read ownership immediately before transition;
- exact expected generation=2 and exact expected stale owner id;
- optimistic-concurrency SHA guard;
- generation increment exactly +1;
- one attempt only;
- immutable evidence of the exception and result;
- after recovery, no special transfer: require the ordinary AWAKE->REARMED->READY->HANDOFF_COMPLETE path;
- stale generations remain fenced on every authoritative side effect.

## Hypothesis update
REVISE: `COLLECT_AT_LEAST_5_GENUINE_OWNER_RENEWAL_GAPS...` is not executable under invocation-scoped ownership without first restoring a live owner.

Next test should be `LIVE_OWNER_REENTRY_CANARY`, then ordinary owner-mediated successor handoff. If ordinary handoff succeeds repeatedly, measure owner tenure/handoff timing from GitHub server timestamps; only introduce a separate heartbeat/renewal primitive if those measurements show it is actually necessary.
