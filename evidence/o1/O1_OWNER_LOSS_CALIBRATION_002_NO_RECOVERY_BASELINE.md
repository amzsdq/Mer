# O1 OWNER LOSS CALIBRATION 002 — NO-RECOVERY BASELINE

Status: IMMUTABLE_SHADOW_ANALYSIS

Current sample 001 already supplies the qualitative no-recovery baseline: durable ownership remained generation 1 while later invocations were SHADOW, so useful authoritative progress could not resume through the owner-mediated path after predecessor loss.

For quantitative comparison, future analysis should measure from the last valid owner server-timestamped liveness/work evidence to the first later authoritative resumed-work evidence. Under the current no-recovery state that latency is effectively unresolved/unbounded until manual/protocol intervention.

Therefore a lease/CAS recovery does not need to beat a fast baseline; it must first convert unbounded owner-loss stall into bounded recovery without violating safety. Only then optimize latency/overhead.
