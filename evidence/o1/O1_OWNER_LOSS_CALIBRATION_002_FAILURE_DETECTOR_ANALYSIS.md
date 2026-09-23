# O1 OWNER LOSS CALIBRATION 002 — FAILURE DETECTOR ANALYSIS

Status: IMMUTABLE_SHADOW_ANALYSIS

## Safety/liveness boundary
The owner-loss problem is a failure-detection problem, not merely an ownership-record problem. With only asynchronous observations, absence of a message/renewal cannot distinguish a dead owner from a delayed owner. Therefore any bounded takeover rule is necessarily a timeout assumption and can trade false takeover risk against recovery latency.

## Consequence for Mer
- Immediate CAS takeover on silence is rejected as unsafe.
- Infinite waiting is rejected as a liveness failure.
- A calibrated bounded non-renewal timeout is a pragmatic failure detector, not proof of death.
- Safety therefore must not depend on the timeout being correct. Safety depends on generation CAS plus per-side-effect fresh owner+generation fencing.
- Timeout error should at worst create a competing stale process that subsequently fails the fence; it must not authorize duplicate authoritative effects.

## Design criterion
Separate the two jobs:
1. Lease/non-renewal timeout provides liveness admission to attempt ownership change.
2. Owner+generation fencing provides safety after ownership change.

This separation is required before the hypothesis can be promoted. It also means expiry calibration should optimize false-takeover rate/recovery latency without being treated as the primary safety mechanism.

## Current verdict
KEEP hypothesis. The recovery mechanism is coherent only with the existing per-side-effect fence. Any proposal to remove the fence because a lease exists should be REJECTED.
