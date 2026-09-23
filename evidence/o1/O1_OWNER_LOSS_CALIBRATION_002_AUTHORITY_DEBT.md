# O1 OWNER LOSS CALIBRATION 002 — AUTHORITY DEBT

Status: IMMUTABLE_SHADOW_ANALYSIS

Current durable ownership still names an old generation-1 predecessor while `spec/execution.json` has evolved to generation 10 experiment metadata. These generations refer to different concepts/domains, but the numerical mismatch can confuse operators and future agents.

Do not infer ownership generation from execution generation. `control/ownership.json` alone owns invocation ownership/generation for the active coordination domain.

Future cleanup should rename or document execution-generation semantics if ambiguity persists, but this wake must not mutate authoritative schema while SHADOW. Record this as documentation debt, not as evidence of authority corruption.
