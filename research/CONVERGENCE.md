# Mer convergence decision

## Decision
Select HYBRID: a small stable execution kernel in the injected prompt plus GitHub-owned dynamic project/program state.

After final validation, add one further invariant:
**dynamic GitHub state must itself have a single runtime authority.**

## Evidence table

| Criterion | PROMPT_HEAVY | HYBRID | POINTER_ONLY |
|---|---|---|---|
| Clean-path bootstrap reads | Best: observed 0 before result | Observed bounded reads in baseline | Similar clean baseline |
| Dynamic mutation | Requires deployed prompt mutation when embedded values change | GitHub-only mutation demonstrated | GitHub-only churn demonstrated |
| Dynamic split-brain risk | Highest because dynamic values are duplicated/deployed | Lowest when dynamic state is GitHub-only **and singularly owned** | Low for dynamic values but pointer is fragile |
| Missing GitHub entrypoint | Embedded work may remain but can stale | Stable kernel carries recovery references/fail-safe | Fail-safe but cannot reconstruct project state from loss of sole pointer |
| Stale deployed prompt | Embedded dynamic values can stale | Stable kernel changes rarely; dynamic values reload from GitHub | Pointer/schema drift remains single-entrypoint weakness |
| Finalization | Can require prompt mutation | One authoritative GitHub program transition | One GitHub transition but weaker recovery path |

## Final authority boundary
Prompt owns only stable execution concerns:
- role/identity and repository/write scope;
- authority/delegation contract;
- canonical recovery references;
- scheduler survival invariants;
- fail-safe behavior;
- work-session discipline and minimum report contract.

GitHub owns dynamic state, but not through multiple equal files:
- `status/program.json` owns stage, next step and active execution pointer;
- `control/active.json` is static bootstrap indirection;
- `spec/execution.json` is valid only when selected by `program.active_execution`;
- historical status belongs in evidence/archive.

## Final Stage 6 gate
PASS:
- clean validation 3/3;
- missing-entrypoint recovery 1/1;
- final stale-state cleanup completed;
- duplicate runtime authority removed.

The smallest supported design is therefore:
**HYBRID stable kernel + GitHub dynamic brain + single authoritative program object.**
