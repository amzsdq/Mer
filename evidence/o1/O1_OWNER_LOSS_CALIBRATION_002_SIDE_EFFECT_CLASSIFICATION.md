# O1 OWNER LOSS CALIBRATION 002 — SIDE-EFFECT CLASSIFICATION

Status: IMMUTABLE_SHADOW_ANALYSIS

For recovery tests classify side effects by fencing strength:

A. Ownership-record CAS: strongest; winner serialized by same-file SHA.
B. Other GitHub mutable state: optimistic concurrency available per file but not atomic with ownership.
C. Immutable evidence writes: safe for SHADOW if uniquely named; not authoritative state.
D. Scheduler mutation: external state, no atomic coupling to GitHub ownership CAS.

Acceptance tests should focus most aggressively on B and D, because successful A alone does not prove system-wide fencing. C is intentionally allowed for concurrent SHADOW research/evidence.
