# Stage 4A — C_POINTER_ONLY synthetic missing-entrypoint test

Candidate: C_POINTER_ONLY / 0.5.0-C-POINTER
Fault model: the only bootstrap entrypoint named by the deployed candidate, `control/active.json`, is treated as missing/unreadable. Canonical repository state is not destroyed.

Observed baseline before fault injection:
- deployed prompt contains repo identity, same-automation scheduler survival rules, write scope, and a fail-safe instruction;
- project/runtime reconstruction begins exclusively from `control/active.json`;
- active.json normally points to Goal, Plan, program, workload, and current stage.

Synthetic fault outcome:
- project/runtime state cannot be reconstructed because no alternate project-state entrypoint is embedded in C_POINTER_ONLY;
- deterministic workload result MUST NOT be invented;
- expected/required behavior is `BOOTSTRAP_FAULT` while preserving the embedded scheduler survival invariants;
- therefore C_POINTER_ONLY is fail-safe but not project-state self-recovering under loss of its sole GitHub entrypoint.

Discriminating implication:
A stable prompt kernel that embeds at least the canonical program/control references can preserve a recovery path that pure pointer-only lacks. This is evidence in favor of HYBRID for resilience, while pointer-only remains viable on the clean path.

Scheduler semantics changed: false.
Canonical state destroyed: false.
Result: PASS as an adverse-behavior sample; architectural weakness observed as expected.
