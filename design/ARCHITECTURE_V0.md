# Mer architecture v0 — Hybrid Bootstrap

## Proposed repository structure

```text
README.md

control/
  active.json
  CANONICAL_PROMPT.md
  prompt-manifest.json
  BOOTSTRAP_CONTRACT.md

spec/
  goal.md
  plan.md
  execution.json
  experiment.json

status/
  current.json
  handoff.json
  scheduler.json

evidence/
  runs/
  trials/
  anomalies/

events/
  events.jsonl

derived/
  frontier.json
  utilization.json
  summary.md

decisions/
  ADR-0001-prompt-repo-boundary.md

research/
  case-study-prompt-github-boundary.md
  experiment-plan.md

archive/
```

## Read path on normal wake
The normal wake should not scan the repository.

1. Reservation prompt is injected.
2. Read `control/active.json`.
3. Read only the referenced canonical kernel/spec/status.
4. Validate schema/generation.
5. Begin useful work.
6. Read evidence/history only on anomaly, decision boundary, or explicit research need.

## active.json concept
```json
{
  "schema_version": 1,
  "kernel": "control/BOOTSTRAP_CONTRACT.md",
  "prompt_manifest": "control/prompt-manifest.json",
  "goal": "spec/goal.md",
  "plan": "spec/plan.md",
  "execution": "spec/execution.json",
  "status": "status/current.json"
}
```

## Desired vs observed
Use an explicit separation.

`spec/execution.json`
- generation
- desired phase
- next action
- runtime policy
- treatment

`status/current.json`
- observed_generation
- actual phase
- last completed action
- work_end_time
- scheduler live state
- failure classification

A worker may claim reconciliation only when `observed_generation == generation` for the relevant transition.

## Prompt propagation
GitHub owns the canonical prompt. The scheduler contains a deployed copy.

This is deliberately **copy + version verification**, not two equal sources of truth:
- canonical source = GitHub
- deployed prompt = executable bootstrap copy

Prompt changes should be rare and structural.
Goal/Plan/state changes should be frequent and GitHub-only.

## Experiment sequence

### A — PROMPT_HEAVY
Baseline: most execution logic embedded in automation prompt.

### B — HYBRID_BOOTSTRAP
Minimal safe bootloader in prompt; dynamic execution state in GitHub.

### C — POINTER_ONLY
Prompt contains little beyond repo and entrypoint.

Compare:
- prompt bytes/tokens
- bootstrap GitHub reads
- seconds to first useful action
- scheduler writes
- prompt writes
- state writes
- stale-policy incidents
- cold-start recovery success
- GitHub-read-failure behavior
- continuation/WAKE success
- useful-work utilization

Run adverse tests:
- canonical prompt updated while deployed prompt is old
- missing/invalid `active.json`
- GitHub read failure
- stale `status`
- conflicting generation
- scheduler update succeeds but wake fails
- worker dies before final close
