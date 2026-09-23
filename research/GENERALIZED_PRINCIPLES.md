# Generalized Principles Registry

Purpose: preserve reusable cross-experiment principles without turning one-off observations into doctrine.

## Status levels
- HYPOTHESIS: plausible generalization, not yet validated.
- PROVISIONAL: supported by multiple clean observations or one strong controlled test, but scope is still narrow.
- VALIDATED_WITHIN_SCOPE: survived the declared controlled/adverse tests in Mer; scope and limits must be stated.
- REJECTED: contradicted by evidence.
- SUPERSEDED: replaced by a better-scoped principle.

## Recording rule
A principle entry must include:
- PRINCIPLE_ID
- STATUS
- CLAIM
- EVIDENCE
- SCOPE
- LIMITS / COUNTEREVIDENCE
- OPERATIONAL_IMPLICATION
- NEXT_FALSIFICATION_TEST

Do not upgrade a principle merely because it is elegant or matches external practice.

---

## P-AUTH-001
STATUS=PROVISIONAL

CLAIM:
Stable behavioral rules placed directly in the deployed prompt appear to have stronger practical enforcement than conflicting repo-only instructions, while explicit delegation in the prompt can transfer authority for dynamic state to GitHub.

EVIDENCE:
- Prior Mer direct-conflict and explicit-delegation experiments.

SCOPE:
- Observed Mer automation/model conditions only.

LIMITS:
- Not a universal undocumented platform precedence theorem.
- Must be rechecked after material prompt/model/runtime changes.

OPERATIONAL_IMPLICATION:
- Keep stable execution/survival invariants in the deployed prompt.
- Keep changing runtime/project state in GitHub under explicit delegation.

NEXT_FALSIFICATION_TEST:
- O2A replication under the current optimizer kernel/model.

---

## P-NATIVE-001
STATUS=HYPOTHESIS

CLAIM:
When a downstream tool has a canonical/native payload syntax, expressing exact invariant constants in that native syntax may improve compliance and reduce translation errors versus an invented alias or prose-only description.

EXAMPLE:
- Native: RRULE:FREQ=HOURLY
- Invented alias: RECURRENCE=HOURLY
- Prose: preserve an hourly recurring schedule

EVIDENCE:
- RFC 5545 defines RRULE:FREQ=HOURLY as native recurrence syntax.
- Tool/function-calling literature shows representation can affect tool-use accuracy.
- No Mer-side controlled result yet.

SCOPE:
- Exact schema/protocol constants only; not nuanced behavioral logic.

LIMITS:
- Native syntax may not help conditional decisions, ownership, recovery, or reasoning.
- More syntax can add token/control overhead or false confidence.

OPERATIONAL_IMPLICATION:
- Test native syntax as a distinct representation candidate.
- Do not promote until controlled Mer fixtures show lower critical error/repair rates without task-quality loss.

NEXT_FALSIFICATION_TEST:
- O2B comparison: natural language vs invented KV DSL vs exact native fragment vs hybrid.

---

## P-EVID-001
STATUS=VALIDATED_WITHIN_SCOPE

CLAIM:
Scheduler write acceptance, live scheduler state, actual later wake, and resumed useful work are distinct evidence states and must not be inferred from one another.

EVIDENCE:
- Repeated Mer/tEST scheduler observations and later-wake verification.

SCOPE:
- Mer relay scheduler experiments.

LIMITS:
- Does not specify the scheduler mechanism itself.

OPERATIONAL_IMPLICATION:
- Record WRITE_OK / STATE_OK / WAKE_OK / WORK_OK separately.

NEXT_FALSIFICATION_TEST:
- Continue adverse missed-wake and accepted-but-no-later-wake tests.

---

## P-STATE-001
STATUS=PROVISIONAL

CLAIM:
Changing runtime state is safer to keep under one durable authority than duplicated across prompt and repository state.

EVIDENCE:
- Prior Mer stale/drift observations between active/program/prompt state.
- Single-authority cleanup reduced ambiguity.

SCOPE:
- Dynamic stage/next_step/execution/hypothesis state.

LIMITS:
- Stable invariants may intentionally be duplicated as canonical source + deployed copy when versioned.

OPERATIONAL_IMPLICATION:
- status/program.json remains the runtime authority for changing program state.

NEXT_FALSIFICATION_TEST:
- O2C version-sync and recovery tests.


---

## P-OVERLAP-001
STATUS=PROVISIONAL

CLAIM:
Concurrent liveness and authoritative ownership are separate dimensions. Multiple invocations may be alive concurrently, while shared authoritative side effects should remain fenced to one durable owner/generation.

EVIDENCE:
- workwork controlled overlap probe observed 198 seconds of concurrent predecessor/successor execution in the same recurring automation.
- workwork later handoff records show concurrent/READY successors but also demonstrate that clean normal authority transfer is not automatic.
- Kubernetes Lease/leader-election design separates multiple live candidates from one holderIdentity and uses optimistic concurrency/version state for ownership.

SCOPE:
- Same-automation overlap relay and shared-state mutation.

LIMITS:
- Mer has not yet reproduced a clean overlap handoff.
- This principle does not establish the optimal overlap lead or ownership timeout.

OPERATIONAL_IMPLICATION:
- Treat overlap as a utilization mechanism, not permission for concurrent authoritative writes.
- Use exactly one ACTIVE_OWNER and one scheduler writer; other live invocations remain SHADOW until fenced transfer.

NEXT_FALSIFICATION_TEST:
- Mer O1 controlled-overlap clean handoff samples with generation/CAS ownership and zero duplicate authoritative side effects.
