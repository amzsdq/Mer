# O1 Owner-Loss Calibration Deadlock — Turn 8

This is SHADOW-prepared immutable evidence, not an authoritative state mutation.

## Observation
The current recovery plan says expiry calibration requires at least five genuine renewal gaps from a valid active OWNER. The durable ownership record, however, still names generation 1 owner `MER-20260923T215700+0900-O1S001`, whose last durable ownership update is from the original sample. Later baton invocations are SHADOW and are prohibited from authoritative renewal or takeover before expiry is calibrated.

## Circular dependency
The current protocol therefore has a bootstrap deadlock:

1. A valid OWNER is required to generate genuine renewal-gap samples.
2. The only durable OWNER is stale/lost.
3. SHADOW cannot become OWNER until owner-loss takeover is admitted.
4. Owner-loss takeover is withheld until expiry is calibrated from genuine OWNER renewal gaps.
5. Therefore the required calibration data cannot be produced under the current rules.

This is not a transient wait. Repeating the same SHADOW wake cannot satisfy the admission condition.

## Revision required
Do not invent an expiry threshold and do not relax generation fencing. Introduce a one-time recovery/bootstrap transition that is explicitly separate from the promoted steady-state lease mechanism. Candidate approaches must be tested as a primary variable, for example:

- manual/administrative generation reset naming one currently live invocation as OWNER, used only to bootstrap renewal calibration; or
- a durable recovery epoch/reset primitive whose safety precondition is stronger than ordinary lease expiry and whose only purpose is to establish a fresh OWNER for calibration.

The bootstrap transition itself must use fresh-read optimistic concurrency, increment generation, and preserve per-side-effect owner+generation fencing. It must not be silently generalized into normal automatic takeover.

## Decision
Current `expiry_threshold = UNSET_REQUIRES_CALIBRATION` remains correct. Current next step is revised from 'wait for a valid OWNER' to 'design and test a bounded bootstrap-owner recovery primitive, then collect genuine renewal gaps'.
