# O1 OWNER LOSS CALIBRATION 002 — TEST VECTORS

Status: PREPARED_NOT_AUTHORIZED

## V1 Healthy transfer
Owner renews/checkpoints and performs normal owner-mediated handoff before expiry.
Expected: recovery path never activates.

## V2 Delayed but alive owner
Owner liveness evidence arrives late but within P+G.
Expected: SHADOW never attempts takeover.

## V3 Owner lost
No valid renewal beyond P+G.
Expected: one candidate CAS generation+1, resumes work.

## V4 Two SHADOW contenders
Both become eligible after same expiry.
Expected: one SHA/CAS winner; loser observes conflict/new generation and stays SHADOW.

## V5 Stale owner resumes after takeover
Old owner attempts authoritative GitHub write.
Expected: fresh generation check rejects it.

## V6 Stale owner scheduler race
Old owner had fresh-read just before takeover and attempts scheduler mutation after generation changes.
Expected: must be explicitly observed/tested; current protocol has cross-system TOCTOU risk. Any conflict prevents promotion.

## V7 Invalid model-clock expiry
Model-authored `updated_at` says expired but server evidence does not.
Expected: no takeover.

## V8 Ambiguous/missing server evidence
Expected: fail closed; no takeover; duration/expiry UNKNOWN where applicable.
