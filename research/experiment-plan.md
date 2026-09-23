# Experiment plan — prompt boundary

## Question
Which split between injected reservation prompt and GitHub durable state gives the best combination of:
- low bootstrap overhead,
- low prompt drift,
- continuation correctness,
- recoverability,
- useful-work utilization?

## Candidates
A. PROMPT_HEAVY
B. HYBRID_BOOTSTRAP
C. POINTER_ONLY

## Freeze rules
Change one primary boundary variable per comparison. Keep scheduler lead policy, task workload class, status schema, and model/reasoning policy fixed during a pairwise test.

## Measurements
- prompt_chars / approximate prompt tokens
- bootstrap file reads
- bootstrap elapsed
- time_to_first_useful_work
- dynamic GitHub reads
- scheduler writes
- prompt mutations
- durable-state mutations
- stale-policy / conflicting-authority incidents
- recovery from missing/stale state
- wake/continuation success
- useful-work utilization

## Initial expectation
B should dominate A on drift/maintenance and dominate C on safe failure/recovery, but this must be tested rather than assumed.
