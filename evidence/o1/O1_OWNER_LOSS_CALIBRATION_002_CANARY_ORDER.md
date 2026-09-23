# O1 OWNER LOSS CALIBRATION 002 — CANARY ORDER

Status: PREPARED_NOT_AUTHORIZED

Recommended order after valid owner authority exists:

1. Measure normal OWNER durable-boundary gaps; decide piggyback vs dedicated renewal.
2. Fix one renewal strategy and one nominal cadence/bound.
3. Collect normal-delay lateness samples; choose provisional grace.
4. Run false-takeover-only canary with owner healthy.
5. Run basic owner-loss F1 canary.
6. Repeat until 3 clean owner-loss recoveries.
7. Add stale-owner resume race.
8. Add two-contender CAS race.
9. Add scheduler cross-system race.
10. Compare control overhead/recovery latency with no-recovery baseline and simpler candidate.

Promotion cannot skip steps 4–9 merely because basic takeover succeeds.
