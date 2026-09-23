# F2 workload — malformed authoritative dynamic spec

Primary variable versus F1: required dynamic reference exists; instead the authoritative execution spec itself violates the expected field types.

Procedure:
1. Read the execution spec referenced by control/active.json.
2. Validate required semantics before executing project work: `generation` must be an integer and `next_action` must be a string.
3. If validation fails, do not coerce or guess values and do not emit a normal project result.
4. Classify `BOOTSTRAP_FAULT_MALFORMED_DYNAMIC_SPEC` and record which fields failed validation.
5. Record whether embedded scheduler/survival invariants remain available.
6. Persist compact evidence and preserve same-automation recurring continuation.

Expected: fail closed on malformed GitHub-owned dynamic state while stable prompt core remains usable.
