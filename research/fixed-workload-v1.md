# Fixed workload v1 — bootstrap boundary benchmark

Purpose: compare prompt-storage architectures without changing scheduler semantics.

On each sample:
1. Read the minimum files required by the candidate.
2. Recover:
   - experiment_id
   - desired_marker
   - current_generation
   - next_action
3. Produce one deterministic useful result:
   `<experiment_id>|<desired_marker>|<current_generation>|<next_action>`
4. Persist a compact trial record.

Do not scan history or evidence before producing the result.
Do not change scheduler lead/cadence as part of this benchmark.

Metrics:
- bootstrap_read_count
- prompt_chars (record from deployed candidate when practical)
- repository_files_needed_before_result
- bootstrap_fault
- chosen values
- scheduler semantics changed? (must be false)
