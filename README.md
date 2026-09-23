# Mer

Research workspace for finding the optimal boundary between a ChatGPT Automation reservation prompt and GitHub durable control/state.

## Result
The research program is COMPLETE.

Selected architecture:
**HYBRID stable kernel + GitHub dynamic brain + single authoritative program object.**

### Prompt owns
- identity and writable repository;
- explicit authority/delegation contract;
- stable bootstrap/recovery references;
- same-automation scheduler safety invariants;
- minimum status/report contract;
- fail-safe behavior;
- model/reasoning policy.

### GitHub owns
- Goal / Plan / current program state;
- dynamic execution and experiment state;
- evidence and decisions;
- canonical prompt/version metadata.

### Single runtime authority
`status/program.json` alone owns:
- current stage;
- next step;
- active execution pointer.

`control/active.json` is a static bootstrap pointer. It must not duplicate dynamic stage/next state.

`spec/execution.json` is authoritative only if selected by `status/program.json.active_execution`.

Historical status belongs in `evidence/` or `archive/`, never as a competing current truth.

## Validated gates
- clean final wakes: 3/3 PASS
- missing-entrypoint recovery: 1/1 PASS
- final authority cleanup: COMPLETE

## Key documents
- `research/FINAL_DESIGN.md`
- `research/CONVERGENCE.md`
- `research/MASTER_PLAN.md`
- `research/case-studies-v1.md`
- `control/CANONICAL_PROMPT_SUPERVISOR.md`
- `status/program.json`

## Sources
Prior internal evidence was read from `amzsdq/RRULEF`, `amzsdq/tEST`, and `amzsdq/workwork` as READ_ONLY during the research program.
