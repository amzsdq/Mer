# O1 OWNER LOSS CALIBRATION 002 — THRESHOLD SELECTION PROCEDURE

Status: PREPARED_NOT_AUTHORIZED

Once real OWNER renewal/checkpoint samples exist for a fixed nominal cadence P:

1. For each consecutive valid server timestamp pair compute interval I_n.
2. Compute positive lateness L_n = max(0, I_n - P).
3. Preserve raw samples; do not hide outliers.
4. Choose provisional grace G only after enough samples to expose normal jitter; small-N maximum is descriptive, not a reliability guarantee.
5. Run a normal-delay false-takeover canary with fixed P,G before intentional loss.
6. If false takeover occurs, REVISE the detector model/threshold; do not simply call it an anomalous success.
7. Measure added control overhead and compare against piggybacked checkpoint option.

No numerical G is selected in calibration 002 because valid OWNER samples are absent.
