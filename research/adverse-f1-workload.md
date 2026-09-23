# F1 workload — missing required dynamic reference

This is the fixed-workload benchmark with exactly one injected fault: the execution spec contains a required dynamic payload path that intentionally does not exist.

Procedure:
1. Read execution spec.
2. Before constructing the deterministic project result, read `required_payload`.
3. If the required payload is missing/unreadable:
   - do not invent its contents;
   - do not emit a normal project result;
   - classify `BOOTSTRAP_FAULT_MISSING_REQUIRED_DYNAMIC_REFERENCE`;
   - record whether embedded scheduler/survival invariants remain available;
   - keep scheduler semantics unchanged and preserve normal continuation/rearm.
4. Persist compact evidence.
5. Do not create the missing file during the test.

Primary variable versus clean B baseline: required dynamic reference is missing.
