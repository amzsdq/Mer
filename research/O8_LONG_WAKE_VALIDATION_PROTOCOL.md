# O8 Long-Wake Validation Protocol

Status: ACTIVE
Goal: prove that one actual nonterminal authoritative Automation invocation can sustain >=600 seconds of genuine useful work without redefining multiple wakes as one turn.

## Clock
- START and END/progress boundaries use GitHub commit server timestamps only.
- Model/local/web time may guide scheduling but cannot establish WORKED.
- `WORKED = END_MARKER.commit_time - START_MARKER.commit_time`.

## Invocation identity
A qualifying sample has one stable `invocation_id` and one authoritative owner tenure. A later Automation wake is a different invocation even if it uses the same canonical automation.

## Useful-work evidence
The sample must contain multiple distinct artifact-backed units. Valid examples include root-cause forensics, mechanism change, controlled differential test, authority audit, protocol repair, and evidence-backed documentation correction. Invalid examples include sleeping, repeated reads with no decision value, reformatting solely to consume time, duplicate converged analysis, or repeated unchanged retries.

## Continuity floor
During the sample:
- canonical automation remains enabled and recurring;
- scheduler state is live-verified after writes;
- substantive authority remains generation-fenced;
- no duplicate authoritative owner is accepted;
- an OPEN legal successor epoch exists before nonterminal exit.

## Stop semantics
Before 600s, voluntary finalization is forbidden unless PROGRAM_COMPLETE becomes valid or a later distinct eligible successor actually completes handoff away. Missing/late successor is not a stop reason. Platform-enforced termination is recorded as evidence, not reclassified as success.

At/after 600s, reaching the duration threshold alone does not require immediate exit. Finish the current smallest useful atomic unit, persist a progress/qualification marker, and continue if the active research plan still has safe useful work and no later successor has taken ownership.

## Qualification record
A qualifying record must bind:
- START commit SHA/time;
- >=600s progress or END commit SHA/time;
- invocation ID and generation;
- list of distinct useful units/artifacts;
- fresh ownership state;
- scheduler live state/evidence;
- whether any successor READY/handoff-away occurred;
- exact elapsed seconds.

## Current sample
- invocation_id: `MER-O8-GEN34-LONGWAKE-20260924`
- generation: 34
- START commit time: `2026-09-24T14:47:51Z`
- threshold time: `2026-09-24T14:57:51Z`
- status: RUNNING
