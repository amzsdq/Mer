# O1 Owner-Loss Lease Calibration Diagnosis — Turn 7

## Observation
The authoritative ownership record is still generation 1 with active_invocation_id `MER-20260923T215700+0900-O1S001`, owner_started_at `2026-09-23T21:57:00+09:00`, and updated_at `2026-09-23T21:57:20+09:00`. It contains no renewable liveness field such as `last_owner_renewal_server_at` or a renewal epoch.

The execution spec requires owner-loss recovery calibration, but `expiry_threshold` is still UNSET and takeover must use server timestamps, fresh-read ownership, generation increment, optimistic concurrency, and per-side-effect fencing.

## Diagnosis
The next experiment cannot validly calibrate an expiry threshold from the existing record. `owner_started_at` and generic `updated_at` are not a bounded recurring liveness signal. Earlier rapid calibration marker commits measure API-write cadence, not natural OWNER checkpoint/renewal cadence. Treating either as lease-renewal data would confound the primary variable and can cause false takeover.

## Minimal testable lease primitive
For the lease comparator only, add exactly one semantic liveness datum to the ownership domain:

- `owner_renewal_seq`: monotonic integer for the current generation.
- The authoritative server time is the GitHub commit timestamp of the ownership-record commit that increments this sequence; do not store or trust a model-authored clock as expiry authority.

Renewal rule:
1. Only the exact current OWNER+generation may renew.
2. Renew only at a natural bounded safe-unit/checkpoint boundary, not by sleeping or synthetic padding.
3. Each renewal is a fresh-read SHA-guarded update of the same ownership record and increments `owner_renewal_seq` only.
4. A SHADOW measures consecutive renewal commit server timestamps to obtain the empirical natural-renewal gap distribution.
5. Expiry calibration begins only after multiple genuine OWNER renewal gaps exist. No threshold is selected from synthetic rapid writes.

## Why this is minimal
A separate heartbeat file duplicates authority and creates a second mutable liveness domain. Reusing the ownership record keeps owner identity, generation, and renewal sequence under one CAS authority. GitHub commit timestamps provide the external clock.

## Discriminating sequence
A. Live-owner calibration: collect genuine renewal gaps while OWNER performs useful work; verify zero false takeover attempts.
B. Freeze renewal intentionally while preserving the candidate SHADOW; after a declared threshold derived from A, attempt one generation+1 SHA-CAS takeover.
C. Verify the predecessor cannot pass the fresh owner+generation fence afterward.
D. Repeat until the promotion gate has 3 clean owner-loss recoveries, with comparator evidence.

## Current result
`REVISE`: lease/non-renewal remains the viable recovery family, but expiry calibration is not yet admissible because the durable authority lacks a genuine recurring renewal signal. Do not choose an expiry number yet.
