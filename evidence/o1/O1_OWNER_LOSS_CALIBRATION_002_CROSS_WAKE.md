# O1 OWNER LOSS CALIBRATION 002 — CROSS-WAKE OBSERVATION

Status: IMMUTABLE_SHADOW_EVIDENCE
Hypothesis: H-O1-OWNER-LOSS-RECOVERY
Primary variable: OWNER_LOSS_RECOVERY_MECHANISM

## Direct server observations
- Prior actual work-session START commit: `47794a2957590bf723e3109d8f2d8ab11c050579`, GitHub `created_at=2026-09-23T16:06:06Z`.
- Prior actual work-session END commit: `6b17a9383c054ecd193780388dbf6305552268c8`, GitHub `created_at=2026-09-23T16:07:19Z`.
- Current actual work-session START commit: `892eaf3203c1a6c0497f626cb3ba364d427bc4aa`, GitHub `created_at=2026-09-23T17:07:02Z`.

Derived server-time intervals:
- Previous completed WORKED = 73 s.
- Previous END -> current START = 3583 s (59m43s).
- Previous START -> current START = 3656 s (60m56s).

## Interpretation
The 60m56s START-to-START interval is a cross-wake observation under the current hourly recurrence, but it is NOT an owner-renewal jitter sample. It combines the configured one-hour recurrence with dispatch/bootstrap/marker overhead. Therefore it must not be used directly as a lease-expiry threshold.

The 59m43s END-to-next-START interval demonstrates the effective idle interval between these two observed work sessions. It is useful for duty-cycle diagnosis but likewise does not isolate provider jitter.

## Calibration consequence
The previous within-wake 7–8 s marker transport samples and this cross-wake ~1h recurrence observation measure different phenomena. They must not be pooled into one expiry distribution. A safe expiry calibration requires a renewable owner liveness marker with a declared expected renewal cadence, then measuring SERVER timestamp lateness relative to that cadence across multiple real wakes.

## Design narrowing
Candidate minimal mechanism:
1. Owner writes immutable renewal marker commits at a declared cadence while authoritative.
2. Each renewal's GitHub server `created_at` is the lease epoch; model-authored time is ignored.
3. Candidate takeover uses age of latest valid owner renewal plus a calibrated grace window.
4. SHA/CAS increments ownership generation exactly once.
5. Every authoritative side effect still fresh-checks owner+generation.

This evidence does not authorize takeover and does not mutate authoritative state.
