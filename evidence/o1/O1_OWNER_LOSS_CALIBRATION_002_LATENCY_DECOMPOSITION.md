# O1 OWNER LOSS CALIBRATION 002 — RECOVERY LATENCY DECOMPOSITION

Status: IMMUTABLE_SHADOW_SYNTHESIS

For owner loss, total idle/recovery delay should be decomposed as:

1. failure-detection window: last valid owner liveness -> expiry eligibility;
2. observation/wake delay: eligibility -> candidate server-time observation;
3. takeover delay: observation -> successful generation CAS;
4. resume delay: CAS -> first resumed useful-work evidence.

Only component 1 is controlled by P/G. Component 2 is controlled by scheduler/overlap strategy. Component 3 by GitHub/CAS path. Component 4 by bootstrap/work admission.

This prevents misattributing a slow recovery to the lease when scheduler wake delay is dominant, or vice versa. Future samples must report these components separately where observable.
