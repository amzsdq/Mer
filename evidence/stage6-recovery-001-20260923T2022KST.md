# Stage 6 final validation — recovery sample 001

- candidate: HYBRID stable prompt kernel + GitHub dynamic state
- stage: 6_FINAL_VALIDATION
- condition: synthetic/reversible missing control/active.json entrypoint
- primary variable: primary bootstrap entrypoint availability
- canonical state was not deleted or mutated for the fault injection
- recovery references used: spec/GOAL.md, research/MASTER_PLAN.md, status/program.json
- authoritative program state recovered: Stage 6; clean 3/3; recovery 0/1; recovery sample required next
- authoritative execution state recovered: generation 4; experiment B_HYBRID_BOOTSTRAP; marker GITHUB_DYNAMIC_CHURN_2; next_action emit_fixed_workload_result
- deterministic_result: B_HYBRID_BOOTSTRAP|GITHUB_DYNAMIC_CHURN_2|4|emit_fixed_workload_result
- invented project state: false
- bootstrap_fault after recovery references: false
- result: PASS

The stable kernel recovered without the primary entrypoint because it embeds canonical recovery references while dynamic project state remained authoritative in GitHub.