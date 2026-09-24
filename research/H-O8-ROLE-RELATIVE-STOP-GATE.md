# H-O8-ROLE-RELATIVE-STOP-GATE

Status: TESTABLE
Stage: O8_LONG_WAKE_USEFUL_WORK_CONTINUATION

## HYPOTHESIS_ID
H-O8-ROLE-RELATIVE-STOP-GATE

## CLAIM
A handoff into this invocation is an ownership-acquisition transition, not this invocation's terminal event. Once the successor wins the generation CAS and becomes ACTIVE_OWNER, the same invocation should continue chaining genuine useful work units until PROGRAM_COMPLETE or a later distinct successor completes handoff away from it.

## PRIMARY_VARIABLE
Stop-gate interpretation after successful ownership acquisition: ROLE_RELATIVE_CONTINUE versus legacy ACQUISITION_IMPLIES_EXIT behavior.

## SOURCE_CLASS
- INTERNAL_EMPIRICAL
- AUTHORITATIVE_IMPLEMENTATION

## SUPPORTING_PRIORS
1. Mer O7/O8 internal evidence: relay continuity converged while observed final invocations remained materially short; premature overall completion was invalidated because long single-wake useful work remained unresolved.
2. AWS Step Functions: an execution advances through states using transitions and terminates only at a terminal state or runtime error; a transition into the next state is not itself execution completion.
3. Kubernetes leader election: multiple contenders may exist, but one successful lease holder becomes the active leader and performs control work while leadership remains valid; acquisition is the start of active authority, not a reason to terminate the acquirer.
4. Temporal durable execution: workflow state is captured and work resumes from durable state after interruption, supporting separation of durable ownership/progress from individual transition events.

## COUNTER_PRIORS / KNOWN_CONFLICTS
1. Mer's previous prompt treated SUCCESSOR_HANDOFF_COMPLETE too broadly, allowing the acquiring invocation to stop immediately after taking ownership. This produced clean relay evidence but failed the long-wake objective.
2. A later distinct successor READY event must still bound current-owner work: once an eligible successor for the current OPEN epoch is READY, the owner finishes only its current atomic unit and yields.
3. Platform invocation/runtime limits may still terminate a correctly continuing owner; role-relative semantics cannot by itself prove >=600s is physically achievable.

## TRANSLATION
- State-machine transition -> handoff changes role/generation, not necessarily invocation termination.
- Leader acquisition -> generation CAS establishes exclusive substantive authority.
- Durable execution -> persist bounded evidence between useful units so interruption does not erase progress.

## NON_TRANSFERABLE_ASSUMPTIONS
- AWS/Kubernetes/Temporal do not establish ChatGPT Automation runtime length or scheduler guarantees.
- Their timing, failure detectors, and execution hosts are not assumed to match Mer.
- >=600s must be demonstrated directly with GitHub-server timestamps in Mer.

## DISCRIMINATING_TEST
On O8 gen33->34 stage entry, the successor performs verified wake-start prearm, records READY, wins a fresh-SHA CAS, and becomes gen34 ACTIVE_OWNER. Without treating that acquisition handoff as a stop gate, the same invocation executes multiple distinct genuine useful units with durable markers/evidence. Continue until either: (a) GitHub START/END timestamps establish WORKED >=600s while multiple genuine units executed and continuity remained secured; (b) a later distinct eligible successor completes handoff away; or (c) platform-enforced interruption occurs, in which case persist evidence and keep the long-wake gate unresolved.

## PROMOTION_GATE
KEEP only if same-invocation post-acquisition work is directly observed and does not violate single-owner fencing/continuity. Overall Mer completion additionally requires the >=600s LONG_WAKE_VALIDATION_GATE.

## REJECTION / REVISION RULE
- REJECT role-relative continuation if acquisition followed by continued work causes duplicate authoritative ownership or violates successor priority.
- REVISE if continuation works but a platform limit repeatedly ends invocations before 600s; document the physical limit and keep the long-work objective unresolved.
- Do not revert to acquisition-implies-exit merely because a short invocation occurs; diagnose the actual stop cause.
