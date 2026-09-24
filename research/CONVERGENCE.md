# Mer prompt-boundary convergence decision

## Scope
This document records convergence of the prompt/GitHub authority-boundary subproblem. It does **not** mean the overall relay-optimization program is complete; current runtime progress is authoritative in `status/program.json` and `research/MASTER_PLAN.md`.

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

## Current program relation
This convergence remains an architectural invariant during the active optimizer program. O1 later validated generation/CAS baton handoff with 5/5 clean samples. O4/O5/O6/O7 established scheduler/handoff/recovery evidence under tested conditions, but the prior overall-completion declaration was invalidated because single-wake useful work remained materially short.

The active stage is O8. O8 adds two constraints that do not change the HYBRID boundary itself:
1. handoff stop gates are invocation-role-relative: acquisition starts the acquiring invocation's owner tenure; only a later handoff away can stop that owner;
2. overall completion requires direct >=600s GitHub-server-timestamp evidence for one nonterminal authoritative invocation containing multiple genuine useful units, unless a platform limit is evidenced and the objective remains unresolved.

A scheduler mismatch incident also hardened the stable prompt kernel: relay self-rearm now uses one normalized absolute DTSTART in a complete recurring VEVENT with exact live readback, and repeated unchanged early-exit retry is replaced by repair-first differential verification.

Overall COMPLETE remains **not satisfied** until the current O8 gates in `status/program.json` and `research/MASTER_PLAN.md` pass.
