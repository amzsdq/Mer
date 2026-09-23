# O1 Owner-Loss Expiry Calibration

Status: DESIGNED_NOT_YET_PROMOTED
Hypothesis: H-O1-OWNER-LOSS-RECOVERY
Primary variable: OWNER_LOSS_RECOVERY_MECHANISM

## Problem
The current ownership record can remain OWNER_ACTIVE after its invocation disappears. Normal owner-mediated transfer therefore has a liveness hole.

## External priors checked
- Kubernetes client-go: LeaseDuration is the interval a non-leader waits after no observed renewal before force-acquire; current core defaults are 15s LeaseDuration / 10s RenewDeadline / 2s RetryPeriod. The implementation explicitly does not itself guarantee fencing and recommends increasing timing margins when API latency requires it.
- etcd: leases are server-managed TTL liveness primitives refreshed by keepalive; election leadership is tied to a lease and transfers when the lease expires/revokes. etcd leader failure detection is timeout-based rather than instantaneous.

These values are NOT transferred to Mer. Only the pattern is prior art.

## Mer observable constraint
Mer does not currently possess a server-managed renewable lease TTL. GitHub commit timestamps are server-authoritative observations, but the existing ownership.json updated_at field is model-authored content and therefore cannot be the expiry clock.

## Candidate lease evidence
Use immutable GitHub commits as renewal markers. The lease epoch is the GitHub server `created_at` of the latest validated OWNER_RENEW marker commit for exact `(active_invocation_id,generation)`. Candidate takeover age must be computed only from GitHub server timestamps.

## Calibration plan
Before choosing expiry threshold E:
1. Collect >=5 ordinary owner renewal/write samples using immutable marker commits.
2. For each sample record server-created timestamp deltas between consecutive owner-observable commits and scheduler wake evidence separately.
3. Derive observed upper jitter J from Mer samples; do not import Kubernetes defaults.
4. Candidate E must exceed the observed legitimate renewal/wake envelope plus a declared safety margin. Exact formula is a hypothesis to test, not yet an invariant.
5. Run false-takeover canary with owner alive but delayed near E. Any takeover while owner is valid => REVISE.
6. Run intentional owner-loss canary. After E with no valid renewal, SHADOW fresh-reads ownership SHA and latest renewal evidence, performs exactly one generation+1 CAS, then fresh-verifies exact owner+generation before every authoritative side effect.

## Safety gates
- No model-authored timestamp may establish expiry.
- No takeover from ownership.json textual updated_at alone.
- Exactly one CAS winner.
- Old owner must fail subsequent fresh owner+generation fence.
- Zero duplicate authoritative side effects and zero scheduler-writer conflicts.
- Three clean owner-loss recoveries plus comparator before promotion.

## Current decision
KEEP the lease-expiry pattern as TESTABLE, but REVISE the initial idea from `ownership.updated_at + threshold` to `GitHub-server-created renewal marker + empirically calibrated threshold`. Expiry threshold remains UNSET until Mer-side samples exist.
