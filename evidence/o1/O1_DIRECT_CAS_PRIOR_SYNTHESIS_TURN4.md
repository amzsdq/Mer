# Direct Successor CAS Prior Synthesis

Status: IMMUTABLE_SHADOW_EVIDENCE

## Internal prior
Mer's workwork intake already establishes that the same recurring automation can run predecessor and successor concurrently and that generation/CAS ownership transfer is the intended safety primitive. It also records that clean normal handoff has not yet been proven.

## External prior
Kubernetes client-go leader election explicitly does not provide fencing by itself. This supports retaining Mer's fresh owner+generation check even if the ownership-acquisition trigger is simplified. GitHub's repository Contents update requires the current blob SHA when replacing a file; this gives Mer a practical optimistic-concurrency guard for a single ownership record, but not a multi-file transaction.

## Competing interpretations
A. LEASE_EXPIRY: successor waits for durable non-renewal then attempts takeover. Advantage: explicit liveness admission. Costs: timeout calibration, heartbeat/renewal state, recovery delay, false-expiry surface.
B. DIRECT_SUCCESSOR_CAS: every deliberately pre-armed successor immediately competes once for generation+1. Advantage: no timeout/heartbeat and owner-loss equals normal handoff. Risk: an invocation not actually authorized as the intended successor could seize ownership.
C. OWNER_MEDIATED_ONLY: safest conceptual transfer but already demonstrated a liveness hole when owner disappears before consuming READY.

## Current best discriminating question
Can successor admissibility be proven structurally from the wake lineage/pre-arm evidence rather than from elapsed lease time? If yes, DIRECT_SUCCESSOR_CAS should be tested first because it is simpler and faster. If not, add a durable successor token/expected invocation identity before adding a clock-based lease; use lease expiry only if structural admission is insufficient.

## No promotion yet
This is prior synthesis only. Mer truth still requires a Mer-side controlled handoff test.
