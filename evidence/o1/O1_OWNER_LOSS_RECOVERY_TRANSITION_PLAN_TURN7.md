# O1 Owner-Loss Recovery Transition Plan — Turn 7

This is SHADOW-prepared immutable evidence, not an authoritative state mutation.

## Required owner-side transition
At the next valid authoritative ownership transition, replace the stale placeholder-only liveness model with a generation-scoped renewal sequence in `control/ownership.json`.

Proposed minimal shape:

```json
{
  "generation": "N",
  "active_invocation_id": "<exact current owner>",
  "status": "OWNER_ACTIVE",
  "owner_renewal_seq": 0
}
```

The clock for each renewal is NOT a JSON timestamp. It is the GitHub server commit timestamp of the SHA-guarded ownership update that increments `owner_renewal_seq`.

## Admission rule
Do not select `expiry_threshold` until at least 5 genuine consecutive renewal gaps from an active OWNER doing useful work are observed. This initial sample count is a calibration admission rule, not a promoted reliability constant.

For the first false-takeover canary, derive a conservative candidate from the observed maximum gap plus separately measured GitHub/API and wake jitter reserve. Keep scheduler jitter and owner renewal jitter as separate measurements; do not fold them into an unexplained constant.

## Failure rules
- Any SHADOW renewal => protocol failure.
- Any renewal without fresh exact OWNER+generation check => protocol failure.
- Any takeover before the declared expiry derived from genuine renewal data => protocol failure.
- Any stale predecessor authoritative write after generation takeover => fencing failure and rejection.
- If natural checkpoint boundaries are not bounded tightly enough to support useful expiry, then add a dedicated renewal only after that failure is measured; do not add heartbeat overhead preemptively.

## Expected simplification
Prefer piggyback renewal on existing required owner checkpoints. Dedicated heartbeat is a fallback only if empirical checkpoint gaps make recovery latency unacceptable or false-takeover risk too high.
