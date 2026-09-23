# O1 OWNER LOSS CALIBRATION 002 — OVERLAP INTERACTION

Status: IMMUTABLE_SHADOW_ANALYSIS

The active overlap strategy prearms a SHADOW successor before nominal owner completion. This has a useful recovery property: the candidate may already be awake/prepared when owner loss becomes detectable, reducing observation/bootstrap delay.

But successor lead=180 s and owner nominal horizon=600 s do not by themselves define a safe lease. A successor waking at owner_start+420 s must not infer owner loss merely because it is present early; it remains SHADOW until normal transfer or independently calibrated expiry admission.

Thus overlap and lease recovery are complementary but distinct variables:
- overlap controls candidate readiness/wake timing;
- lease/non-renewal controls recovery admission;
- generation fencing controls authority safety.

Keep these dimensions separate in measurements.
