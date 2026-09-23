# B HYBRID baseline — sample 001

- candidate: B_HYBRID_BOOTSTRAP
- prompt_version: 0.3.0-B-HYBRID
- workload: research/fixed-workload-v1.md
- deterministic_result: `B_HYBRID_BOOTSTRAP|GITHUB_DYNAMIC|1|emit_fixed_workload_result`
- bootstrap_read_count_before_result: 4
- repository_files_needed_before_result: control/active.json, status/current.json, research/experiment-matrix-v1.md, spec/execution.json
- bootstrap_fault: false
- scheduler_semantics_changed: false
- note: Current deployed ON_WAKE requires active control plus current execution/status/workload references. This sample deliberately did not scan history/evidence before producing the result.
