# Live baseline — 2026-09-23

## Directly observed automation families

### PROMPT_HEAVY
Current RRULEF and tEST automation prompts embed extensive execution protocol, scheduler mutation semantics, evidence taxonomy, reporting contract, and dynamic experiment-specific behavior.

Observed strengths:
- low ambiguity after wake if prompt is current;
- scheduler safety remains available even if repository reads fail.

Observed costs/risks:
- large injected prompt every wake;
- policy duplicated between prompt and repository;
- prompt mutation required when embedded operational policy changes;
- stale-copy/conflicting-authority risk.

### POINTER_ONLY
Historical R worker/foreman wake-pointer prompts demonstrate a minimal pointer pattern: identity/root/source-of-truth plus instruction to CONFIG_REFRESH from GitHub.

Observed strengths:
- tiny injected prompt;
- dynamic policy changes propagate through durable state without prompt rewrite.

Observed costs/risks:
- safe behavior depends heavily on successful bootstrap reads;
- insufficient local safety kernel can make repository-read/bootstrap faults ambiguous;
- prior R history contains repeated expansion from thin pointers back toward embedded execution details, suggesting pure pointer-only can be operationally fragile when scheduler semantics themselves matter.

### HYBRID_BOOTSTRAP
Mer candidate keeps identity, entrypoint, scheduler safety invariants, fail-safe semantics and compact report contract in the injected prompt, while Goal/Plan/current task/runtime policy/evidence remain durable.

## Initial comparative verdict
HYBRID_BOOTSTRAP remains the leading candidate, but this is not promotion evidence yet. Direct controlled trials must hold scheduler semantics/workload fixed and vary only the storage boundary.

## Design correction found during baseline
The canonical prompt should not say to read a fixed list of desired spec/status files that may not exist. Normal bootstrap should read `control/active.json`, then only the references declared there. This keeps the injected prompt stable while allowing the durable graph to evolve.

## Next discriminating test
Instrument bootstrap cost and recovery behavior for HYBRID_BOOTSTRAP, then run controlled fault cases:
1. normal fresh bootstrap;
2. canonical prompt newer than deployed version;
3. missing/invalid active.json;
4. stale/conflicting observed generation;
5. scheduler state valid while project state bootstrap fails.
