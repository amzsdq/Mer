# O1 Authority/Liveness Invariant

## Finding
The current O1 execution is structurally non-progressing, not merely under-sampled.

Current execution requires at least five genuine renewal gaps from the authoritative OWNER before deriving an expiry threshold. Current ownership still names generation 2 invocation MER-20260924T060808+0900-O1BOOT001 with owner_renewal_seq=1. Later wakes are SHADOW and therefore cannot manufacture genuine OWNER renewals without violating the authority model.

## Derived invariant
A calibration procedure MUST NOT require future observations that can only be produced by an authority actor that the same durable state no longer has a supported way to execute.

Equivalently, recovery calibration has a prerequisite:

LIVE_CALIBRATABLE_OWNER OR EXPLICIT_RECOVERY_AUTHORITY

If neither exists, the experiment is structurally blocked and repeated SHADOW observations do not increase the relevant sample size.

## Minimal correction
For a stuck generation whose named OWNER cannot renew, the ordering should be:
1. durable program/execution authority explicitly opens one single-use recovery transition for the exact stuck generation;
2. an eligible READY invocation performs fresh-read optimistic-concurrency generation increment;
3. old generation remains fenced;
4. the new live OWNER emits genuine renewals;
5. only then derive expiry/failure-detector parameters from server-timestamped renewal gaps.

This is not permission for ad-hoc SHADOW self-promotion. Until status/program.json/spec execution authorizes such a recovery transition, SHADOW remains read/prepare/immutable-evidence only.

## Decision
REVISE the experimental ordering. Do not count scheduler wakes as owner renewals, do not invent an expiry threshold, and do not repeat passive observations as if they advance GENUINE_OWNER_RENEWAL_GAP_DISTRIBUTION.
