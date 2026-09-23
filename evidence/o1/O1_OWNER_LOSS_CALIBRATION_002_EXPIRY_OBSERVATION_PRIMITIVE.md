# O1 OWNER LOSS CALIBRATION 002 — EXPIRY OBSERVATION PRIMITIVE

Status: IMMUTABLE_SHADOW_ANALYSIS

## Issue
The design needs a fresh GitHub-server timestamp T to evaluate age of the latest renewal. Reading an old commit returns its timestamp but does not itself supply a new server time.

## Minimal observable primitive
A candidate can create an immutable observation marker and use that marker commit's GitHub server `created_at` as T. Then:

observed_age = OBSERVATION_MARKER.created_at - LATEST_VALID_RENEWAL.created_at

This satisfies the server-time rule but costs one write per expiry check.

## Cost/simplicity consequence
Polling expiry through repeated observation-marker writes would be wasteful. Prefer evaluating once per actual candidate wake/boundary where a GitHub write is already required, or piggyback T on an immutable candidate READY/evidence marker. Do not create high-frequency clock-only commits.

## Canary rule
For takeover admission, the exact observation marker used as T must be retained in sample evidence so the age calculation is reproducible.
