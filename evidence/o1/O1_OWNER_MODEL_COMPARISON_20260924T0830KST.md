# O1 owner-model comparison

Question: after successor-token delivery was rejected under current primitives, compare a durable logical owner against a first-valid-contender ordinary handoff.

Current facts:
- generation already serves as monotonic fencing state;
- every authoritative side effect requires a fresh ownership read;
- the generation-2 owner is an ended invocation, so it cannot renew or mediate transfer;
- requiring five renewals before any recovery expiry creates a circular dependency in the current state.

## A. Durable logical owner
Using the automation identity as the owner would let later wakes share one identity, but concurrent invocations of that same automation would then be indistinguishable for owner-only writes. Keeping per-invocation generation fencing fixes that ambiguity but restores the need to transfer invocation authority. Letting every SHADOW renew the logical lease is also unsound for liveness because scheduler wakes could keep a substantively inactive holder alive indefinitely.

Verdict: REJECT as primary substantive authority. Durable automation identity may be a namespace/member identity only.

## B. First-valid-contender ordinary handoff
Use an OWNER-opened, generation-bound handoff epoch rather than a predicted future invocation identity.

Candidate protocol:
1. OWNER at generation G opens handoff epoch G when a successor is needed.
2. A SHADOW becomes eligible only after verified pre-arm, checkpoint reconstruction, and durable READY evidence.
3. Eligible READY contenders fresh-read ownership and epoch and attempt one optimistic-concurrency G to G+1 CAS.
4. Exactly one winner becomes active; concurrent losers fail on stale SHA/generation.
5. All later authoritative writes remain protected by fresh active-invocation plus generation fencing.
6. No elapsed-time condition participates in this ordinary-handoff experiment.

This is first valid contender, not first wake. The OWNER-opened epoch supplies transfer intent; READY supplies admission; CAS selects one winner. A shared token adds no safety if every contender can read it.

Abnormal owner loss should remain a separate mechanism that may later use a renewable lease/failure detector. Separating ordinary handoff from owner-loss recovery removes the present circular dependency.

Decision: PROMOTE B as the next design hypothesis; REJECT A as primary authority.

This file is immutable SHADOW evidence only. It does not revise status/program.json, spec/execution.json, or control/ownership.json.

Next authorized experiment variable: OWNER_OPENED_HANDOFF_EPOCH_WITH_FIRST_READY_CAS.
Tests: one READY contender; two concurrent READY contenders; non-READY rejection; CLOSED-epoch rejection; stale predecessor write rejection after G+1.
