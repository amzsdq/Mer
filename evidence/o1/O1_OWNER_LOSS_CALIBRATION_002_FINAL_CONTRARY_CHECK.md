# O1 OWNER LOSS CALIBRATION 002 — FINAL CONTRARY CHECK

Status: IMMUTABLE_SHADOW_EVIDENCE

Contrary cases retained rather than optimized away:
- delayed owner can look dead;
- lease does not fence stale owner;
- GitHub CAS does not atomically fence scheduler;
- piggyback checkpoint cadence may be workload-unbounded;
- hourly recurrence can dominate recovery observation latency;
- dedicated renewal can reduce useful-work duty cycle;
- successful takeover without resumed useful work is insufficient.

None of these currently falsifies the fallback lease/CAS hypothesis outright, but each has an explicit canary/rejection condition. Hypothesis remains KEEP_REVISED, not promoted.
