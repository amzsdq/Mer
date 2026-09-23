# ADR-0002 — Single runtime authority and non-atomic finalization

## Status
ACCEPTED — 2026-09-23

## Context
Final validation completed successfully, but four files represented different eras of the program:
- `status/program.json`: COMPLETE
- `control/active.json`: Stage 6 / CLEAN-003
- `status/current.json`: Stage 3 HYBRID baseline
- `spec/execution.json`: generation 4 active HYBRID execution

This contradicted the final architecture's claim that GitHub is the authoritative dynamic brain.

## Decision
1. `status/program.json` is the single runtime authority for stage, next step and active execution.
2. `control/active.json` becomes static bootstrap indirection and cannot duplicate dynamic runtime fields.
3. `spec/execution.json` is authoritative only when selected by `program.active_execution`.
4. Retired current-state snapshots move to `archive/`.
5. Program completion is one authoritative program-state transition, not a best-effort multi-file atomic update.

## Why
GitHub Contents API writes to separate files are independent. A design that requires several files to transition simultaneously creates an avoidable partial-update/split-brain failure mode.

By making a single program object authoritative, cleanup can fail without changing the truth of whether the program is RUNNING or COMPLETE.

## Consequences
- normal bootstrap requires one explicit runtime authority;
- helper files can be stale only as non-authoritative maintenance debt, not competing instructions;
- historical execution/status remains auditable in archive/evidence;
- reopening the research program requires an explicit new program-state transition.
