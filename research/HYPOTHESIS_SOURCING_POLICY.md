# Hypothesis Sourcing Policy

Status: REQUIRED
Purpose: prevent unsupported theory-first research.

## Rule
A new Mer hypothesis must not be promoted directly from intuition.

Before a hypothesis becomes TESTABLE, perform a bounded prior-art scan covering, as applicable:
1. internal empirical evidence: Mer, amzsdq/tEST, amzsdq/workwork;
2. authoritative production documentation from systems implementing the relevant primitive;
3. peer-reviewed or otherwise technically serious research when the question has an established literature;
4. contrary evidence or a competing mechanism, not only supporting references.

## Hypothesis record
Each hypothesis must record:
- HYPOTHESIS_ID
- CLAIM
- PRIMARY_VARIABLE
- SOURCE_CLASS
- SUPPORTING_PRIORS
- COUNTER_PRIORS / KNOWN_CONFLICTS
- TRANSLATION: what invariant/mechanism from the source maps to Mer
- NON_TRANSFERABLE_ASSUMPTIONS: what cannot be assumed to carry over
- DISCRIMINATING_TEST
- PROMOTION_GATE
- REJECTION / REVISION RULE

## Source classes
- INTERNAL_EMPIRICAL: direct tEST/workwork/Mer evidence.
- AUTHORITATIVE_IMPLEMENTATION: official docs/design of mature systems.
- ACADEMIC: peer-reviewed/formal research.
- EXPLORATORY_UNSOURCED: no useful prior art found after a bounded search.

EXPLORATORY_UNSOURCED is allowed only when:
- the search attempt is recorded;
- the hypothesis is explicitly labeled exploratory;
- it is not treated as a default or promoted on weaker evidence than sourced alternatives.

## Evidence discipline
References are not proof that ChatGPT Automations behave the same way.
Use references to derive:
- invariants,
- failure modes,
- measurement ideas,
- competing designs,
- adverse tests.

Then validate those claims empirically in Mer.

## Search stopping rule
Do not turn every hypothesis into an open-ended literature review.
Stop the prior-art scan when enough evidence exists to define:
- at least one plausible mechanism,
- at least one competing/failure interpretation,
- a discriminating test.

Broaden only when the experiment result is ambiguous or materially contradicts the current model.
