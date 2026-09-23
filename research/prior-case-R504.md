# Prior case study — amzsdq/R issue #504

## Scope
READ_ONLY recovery of the historical PROMPT-EMBEDDED vs DURABLE-LOADED protocol A/B.

## Experimental variants
A — EMBEDDED_STABLE
- stable P1/P2/P3 wake/reservation/work-time protocol embedded in the automation prompt
- fresh GitHub still used for assignment/root evidence
- normal wake did not reload the stable protocol bundle

B — DURABLE_LOADED
- automation prompt held actor/root/protocol references
- every wake loaded fresh GitHub protocol bundle + wake runbook before substantive work

## Observed results

### A
- 160 distinct findings / 16 batches
- bootstrap GitHub reads before first substantive work: 1
- protocol load/parse failures: 0 by construction
- schedule mutations: 3
- collision/stale-wake: none observed
- premature return while runnable backlog remained: no

### B
- 140 distinct findings / 14 batches
- bootstrap GitHub reads before first substantive work: 2
- policy load/parse failures: 0
- schedule mutations: 2
- collision/stale-wake/missed continuation: none observed
- useful span 285.4s; stopped because analytic scope saturated, not due premature return

## Historical judge decision
The historical judge recommended:

HYBRID_STABLE_CORE_WITH_DURABLE_DYNAMIC_STATE

Reason:
- embedded stable core measurably saved one recurring bootstrap read and removed one stable-policy load/parse dependency;
- durable-loaded policy had superior immediate freshness/change propagation;
- unversioned embedded policy was rejected because of stale-kernel/fleet-skew risk;
- dynamic authority/task/recovery/verification state should remain fresh durable;
- stable execution mechanics could be embedded only when versioned/hashed and compatibility-checked.

## Relevance to Mer
This is strong prior evidence against rerunning a naive full A/B from zero. Mer should treat HYBRID as the prior-leading candidate and spend new experiments on unresolved questions:
1. instruction force/priority when direct embedded instruction conflicts with later-loaded GitHub content;
2. whether explicit delegation from prompt to GitHub makes the durable value reliably authoritative inside that domain;
3. minimum safe bootstrap kernel size;
4. prompt drift detection/repair cost;
5. failure behavior when GitHub bootstrap is missing/malformed/unavailable.

## Evidence limitation
The historical A/B did not establish a platform-level instruction-precedence theorem. It measured operational behavior and overhead in specific automation runs. Mer must not generalize "prompt always wins" from it.
