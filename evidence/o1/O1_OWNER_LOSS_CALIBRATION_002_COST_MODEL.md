# O1 OWNER LOSS CALIBRATION 002 — COST MODEL

Status: IMMUTABLE_SHADOW_ANALYSIS

## Recovery mechanism overhead
A renewable lease adds control-plane writes. If renewal period is P seconds during active owner time W, approximate renewal writes per owner session are ceil(W/P). This overhead competes directly with useful-work duty cycle.

## Experimental objective
Do not optimize expiry latency alone. Measure:
- renewal write count/time,
- useful work displaced by renewal/control operations,
- false takeover rate,
- owner-loss recovery latency,
- duplicate authoritative effects / scheduler conflicts.

## Candidate trade-off
Short P:
- faster evidence of continued liveness and potentially faster recovery,
- more GitHub writes/control overhead.

Long P:
- lower overhead,
- slower owner-loss detection and larger idle gap after failure.

Because normal healthy handoff remains owner-mediated, renewal exists only to support fallback failure detection. The eventual production design should use the longest P that still meets acceptable recovery latency, subject to measured false-takeover behavior.

## Simplicity pressure
If experiments show owner loss is rare and lease overhead materially reduces useful-work duty cycle, consider a cheaper external liveness/watchdog signal rather than continuous owner renewal. That is a competing hypothesis, not yet adopted.
