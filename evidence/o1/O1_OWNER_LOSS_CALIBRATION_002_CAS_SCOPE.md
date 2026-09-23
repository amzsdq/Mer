# O1 OWNER LOSS CALIBRATION 002 — CAS SCOPE

Status: IMMUTABLE_SHADOW_ANALYSIS

GitHub Contents API SHA protection gives optimistic concurrency for the specific ownership file version. It can ensure two candidates updating the same `control/ownership.json` version do not both win that file update.

It does not atomically cover:
- status/program.json,
- spec/execution.json,
- scheduler state,
- arbitrary evidence files.

Therefore takeover transaction should be conceptually minimal: first CAS only the ownership record/generation, then treat all subsequent state/scheduler actions as post-takeover operations guarded by fresh ownership checks. Do not attempt a pseudo-transaction across multiple files and assume atomicity.

This reduces the critical section and makes winner determination unambiguous.
