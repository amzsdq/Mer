# Priority / force hypothesis

## Hypothesis
Injected automation-prompt instructions may exert stronger and more reliable control than equivalent instructions loaded later from GitHub.

This is plausible but must not be assumed as a platform guarantee. Test behavioral outcomes.

## Key distinction
"Prompt wins" and "GitHub wins" are not binary architecture choices.

Three cases:
- P_DIRECT: automation prompt directly specifies behavior X; GitHub specifies conflicting behavior Y.
- P_DELEGATED: automation prompt explicitly delegates a field/domain to fresh GitHub; GitHub specifies Y.
- POINTER_ONLY: automation prompt only points to GitHub; GitHub specifies Y.

Expected:
- P_DIRECT: embedded instruction likely dominates for hard invariants.
- P_DELEGATED: GitHub should dominate within the explicitly delegated domain.
- POINTER_ONLY: low prompt drift but weaker cold-start/failure containment.

## Safe test fields
Use harmless markers and status formatting only. Never use destructive actions or scheduler disable/create as conflict payloads.

## Architecture implication if confirmed
Embed only hard invariants and bootstrap semantics in the prompt. Explicitly delegate Goal/Plan/dynamic policy to GitHub. Do not duplicate the same dynamic field in both places.
