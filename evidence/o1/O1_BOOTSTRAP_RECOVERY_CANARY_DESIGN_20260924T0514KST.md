# O1 one-time bootstrap recovery canary design

Status: TESTABLE_CANDIDATE_ONLY — NOT EXECUTED
Date: 2026-09-24
Primary variable: BOOTSTRAP_RECOVERY_FROM_UNCALIBRATED_STALE_OWNER

## Problem
Mer is in a calibration deadlock. The durable owner record names generation 1 owner `MER-20260923T215700+0900-O1S001`, but the active recovery experiment requires calibrated durable non-renewal before takeover and the expiry threshold is explicitly unset. A valid OWNER is needed to collect genuine renewal gaps, while the stale owner must be replaced before a valid OWNER can exist.

## Minimal one-time canary
This is NOT the production lease rule and MUST NOT establish a reusable timeout constant.

Admission requires all of the following from fresh durable reads:
1. `control/ownership.json` is unchanged at generation 1 and the same `active_invocation_id`.
2. `status/program.json` still names `H-O1-OWNER-LOSS-RECOVERY` and owner-loss recovery remains under test.
3. `spec/execution.json` still has `expiry_threshold=UNSET_REQUIRES_CALIBRATION` and requires generation increment + optimistic concurrency + per-side-effect fencing.
4. The current candidate has successfully performed its wake-start re-arm and prepared the immediate next action.
5. No newer durable owner renewal/transfer evidence is discovered before the CAS.

Action if admitted:
- Perform exactly one SHA-guarded update of `control/ownership.json` from generation 1 to generation 2 naming the current candidate as owner.
- Mark the transition explicitly as `BOOTSTRAP_RECOVERY_CANARY`, not normal lease expiry.
- Initialize a generation-scoped `owner_renewal_seq=0`; subsequent genuine safe-unit/checkpoint renewals increment it.
- Immediately fresh-read ownership and require exact candidate id + generation 2 before any authoritative mutation.
- If CAS loses/conflicts, remain SHADOW; do not retry unchanged in the same wake.

Safety property
A stale generation-1 invocation remains physically possible, so every authoritative shared-state side effect must continue to fresh-check exact owner+generation. This canary does not claim lease safety; it only breaks the bootstrap circular dependency under an explicit experimental exception.

Post-canary experiment
1. Collect >=5 genuine OWNER renewal intervals using GitHub-accepted server/commit timestamps, not model-authored clocks.
2. Derive an expiry candidate from observed distribution plus explicitly declared safety margin; do not reuse the bootstrap age as the lease threshold.
3. Run healthy-owner false-takeover canary.
4. Run intentional owner-loss takeover canary.
5. Run two-candidate race and stale-owner fencing checks.
6. Require 3 clean owner-loss recoveries plus comparator before production promotion.

Reject / abort conditions
- Any evidence that generation 1 owner renewed or transferred after the fresh admission read.
- Any ambiguous authority reconstruction.
- CAS conflict/loss.
- Any duplicate authoritative side effect after takeover.
- Any attempt to generalize this one-time bootstrap exception into a production expiry rule without calibration.

Decision
KEEP AS TESTABLE ONE-TIME CANARY. This is simpler and safer than inventing an uncalibrated production lease timeout, while preserving generation fencing and optimistic concurrency. Execution requires an explicit durable experiment state authorizing this bootstrap exception before the ownership CAS.