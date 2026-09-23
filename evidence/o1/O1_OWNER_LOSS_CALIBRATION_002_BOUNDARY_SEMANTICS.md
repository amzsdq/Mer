# O1 OWNER LOSS CALIBRATION 002 — BOUNDARY SEMANTICS

Status: IMMUTABLE_SHADOW_ANALYSIS

A 'safe unit boundary' for duration enforcement should not imply that every boundary needs a new standalone file. It means a point after a bounded useful unit where state/evidence is sufficiently durable to recover and where the next unit can be admitted safely.

For SHADOW research, several related analyses may be consolidated into one immutable evidence artifact if they form one bounded unit. For OWNER work, authoritative state updates remain governed by declared ownership and CAS rules.

This interpretation avoids turning the 600-second floor into a file-generation incentive. The floor controls voluntary termination, not artifact granularity.
