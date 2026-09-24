# Mer

Research workspace for finding the optimal boundary between a ChatGPT Automation reservation prompt and GitHub durable control/state.

## Current status
The research program is ACTIVE. Prompt-boundary architecture convergence is retained, while relay operating-policy optimization is currently in Stage O4 (wake-prearm timing).

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
- prompt-boundary architecture convergence: PASS
- O1 controlled-overlap viability/stabilization: 5/5 clean handoffs PASS
- O1→O4 stranded-owner recovery: PASS (gen8→gen9)
- current stage: O4 adaptive pre-arm optimization
- overall program completion: NOT YET; only O7 may mark COMPLETE

## Key documents
- `research/FINAL_DESIGN.md`
- `research/CONVERGENCE.md`
- `research/MASTER_PLAN.md`
- `research/case-studies-v1.md`
- `control/CANONICAL_PROMPT_SUPERVISOR.md`
- `status/program.json`

## Sources
Prior internal evidence was read from `amzsdq/RRULEF`, `amzsdq/tEST`, and `amzsdq/workwork` as READ_ONLY during the research program.
