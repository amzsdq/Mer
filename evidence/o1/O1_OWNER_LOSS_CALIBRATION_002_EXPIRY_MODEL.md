# O1 OWNER LOSS CALIBRATION 002 — EXPIRY MODEL

Status: IMMUTABLE_SHADOW_ANALYSIS
Hypothesis: H-O1-OWNER-LOSS-RECOVERY

## Problem
An expiry threshold cannot be calibrated from arbitrary GitHub write latency or from hourly wake spacing. It must be defined against an expected owner-renewal process.

## Observable model
Let each valid owner renewal be an immutable GitHub object with server `created_at` R_n. Let the declared nominal renewal period be P. For consecutive valid renewals define observed lateness:

L_n = (R_n - R_(n-1)) - P

Only positive lateness contributes to false-expiry risk. The candidate grace G must be derived from empirical upper-tail positive lateness plus an explicit safety margin; until enough real samples exist, G remains UNSET.

Takeover admission at a fresh server-observable time T is conceptually:

T - latest_valid_owner_renewal.created_at >= P + G

No model-authored timestamps participate.

## Safety conditions
- Renewal is valid only if it names the exact current owner and generation observed by a fresh ownership read before renewal publication.
- A stale owner's later marker after generation changes is invalid for the new generation.
- Candidate must fresh-read ownership immediately before SHA/CAS takeover.
- Takeover advances generation once; failed CAS returns candidate to SHADOW/read path.
- Every later authoritative side effect fresh-checks exact owner+generation; lease expiry itself is not fencing.

## Calibration plan
A. Choose one declared renewal cadence P for the canary; do not vary P and grace simultaneously.
B. Collect multiple real-wake server timestamps for valid renewals without intentional owner loss.
C. Compute positive lateness distribution and choose a provisional G conservatively.
D. Run false-takeover canary under normal delay first.
E. Only after zero false takeover, run intentional owner-loss takeover sample.

## Current decision
KEEP the lease-expiry/CAS hypothesis but REVISE calibration semantics: expiry is based on lateness relative to a declared renewal cadence, not raw marker latency and not raw recurrence spacing. No expiry threshold is promoted yet.
