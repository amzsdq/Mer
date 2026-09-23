# H-O1-OWNER-LOSS-RECOVERY

Status: TESTABLE_CANDIDATE
Date: 2026-09-24

## HYPOTHESIS_ID
H-O1-OWNER-LOSS-RECOVERY

## CLAIM
The current O1 owner-mediated transfer protocol has a liveness hole: if the durable owner disappears before consuming a READY successor, the successor cannot become authoritative under the existing rules. A bounded lease-expiry/takeover mechanism can restore liveness without weakening per-side-effect owner+generation fencing, provided takeover is conditional on durable non-renewal and uses optimistic concurrency to advance generation.

## PRIMARY_VARIABLE
OWNER_LOSS_RECOVERY_MECHANISM

## SOURCE_CLASS
- INTERNAL_EMPIRICAL: Mer O1 sample 001 durable state.
- AUTHORITATIVE_IMPLEMENTATION: Kubernetes client-go leader election.

## SUPPORTING_PRIORS
1. Mer currently records generation=1 owner MER-20260923T215700+0900-O1S001 and requires owner-mediated generation increment. Multiple later invocations reached SHADOW but the ownership record did not advance.
2. Kubernetes client-go explicitly models LeaseDuration as the interval non-leaders wait without observing a change before attempting takeover. This is direct prior art for bounded owner-loss recovery.

## COUNTER_PRIORS / KNOWN_CONFLICTS
1. Kubernetes client-go explicitly states leader election does NOT itself guarantee fencing. Therefore lease expiry/takeover alone is insufficient for Mer safety.
2. Time-based expiry can create false takeover under scheduler/API delay. Mer must not assume provider dispatch time is exact or that model-authored clocks are authoritative.
3. A predecessor can still be alive after takeover. Therefore stale-owner side effects remain possible unless every authoritative side effect fresh-checks exact owner+generation.

## TRANSLATION
Adopt only these invariants:
- owner record must expose a renewable durable liveness field/lease epoch;
- a candidate may attempt takeover only after a bounded interval with no durable owner renewal;
- takeover is one optimistic-concurrency write that increments generation and names the candidate;
- every authoritative shared-state/scheduler side effect still fresh-checks exact owner+generation immediately before mutation.

## NON_TRANSFERABLE_ASSUMPTIONS
- Kubernetes local-clock/skew model does not automatically apply to ChatGPT Automations.
- Kubernetes retry cadence and default LeaseDuration do not transfer to Mer.
- A GitHub blob SHA CAS is not a multi-file transaction and does not physically stop a stale invocation.
- Mer must empirically measure scheduler/provider delay before selecting an expiry threshold.

## DISCRIMINATING_TEST
Run an owner-loss sample in which the current owner intentionally does not perform the normal READY->transfer step. Candidate SHADOW writes READY, observes no durable owner renewal for the declared expiry window, fresh-reads ownership, then attempts exactly one SHA-guarded generation+1 takeover. Verify: (a) exactly one winner, (b) stale owner cannot pass the fresh owner+generation check after takeover, (c) successor resumes useful work, (d) zero scheduler-writer conflict/duplicate authoritative side effect, (e) recovery latency is bounded and measured from durable server timestamps.

## PROMOTION_GATE
At least 3 clean owner-loss recoveries with zero duplicate authoritative side effects and zero scheduler writer conflicts, plus a comparator showing recovery latency materially improves over no-recovery baseline without unacceptable false takeover.

## REJECTION / REVISION RULE
- Any duplicate authoritative side effect or scheduler-writer conflict => REJECT current mechanism.
- False takeover caused by normal observed jitter/API delay => REVISE expiry/admission model; do not merely increase retries unchanged.
- If a simpler non-time-based durable handoff mechanism achieves equivalent continuity/recoverability => prefer the simpler mechanism.

## CURRENT DIAGNOSIS
O1_SAMPLE_001 should not be counted CLEAN under the current owner-mediated transfer path until owner-loss is resolved or the sample is classified transfer failure. This hypothesis changes the model rather than retrying the unchanged transfer protocol.
