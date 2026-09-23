# O1 OWNER LOSS CALIBRATION 002 — REJECTION CRITERIA

Status: IMMUTABLE_SHADOW_SYNTHESIS

Reject the current lease/CAS recovery design, not merely retune it, if any of these occur under correctly instrumented tests:
- two candidates both become authoritative for the same generation transition;
- stale predecessor can perform duplicate authoritative GitHub effects after takeover despite required fencing;
- scheduler conflicts cannot be bounded/reconciled sufficiently to meet zero-conflict promotion gate;
- server-time evidence needed for expiry is not reliably obtainable without prohibitive write/control overhead;
- recovery path materially reduces healthy-path continuity/utilization versus a simpler mechanism with equivalent recovery;
- ambiguity forces routine fail-closed stalls comparable to the original no-recovery problem.

Retune/revise rather than reject if only P/G produces premature/slow detection while safety fences hold.
