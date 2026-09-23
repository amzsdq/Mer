# O1 OWNER LOSS CALIBRATION 002 — RENEWAL SCHEMA CANDIDATE

Status: PREPARED_NOT_AUTHORIZED

Minimal immutable renewal marker fields:
- schema_version
- experiment_id
- sample_id
- owner_invocation_id
- generation
- sequence
- ownership_blob_sha_observed_before_publish

Explicitly excluded as authority:
- model-authored current_time
- model-authored expires_at
- model-authored elapsed

The marker's authoritative lease epoch is the GitHub server timestamp attached to the commit/object after publication.

Validation before a renewal can count:
1. Fresh-read `control/ownership.json`.
2. Require exact owner_invocation_id and generation match.
3. Record observed ownership blob SHA in immutable marker.
4. Publish marker.
5. Later consumers accept it only for that exact generation.

This schema avoids updating one hot lease file repeatedly and preserves an audit trail. Cost: one GitHub write per renewal. A mutable single lease file would reduce file count but still require writes and would erase direct immutable renewal history; therefore immutable markers are preferred for the experiment, not yet promoted for production.
