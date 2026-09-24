# O8 related prior — R RRULE T3 wake provenance

Source: amzsdq/R issue #504, historical T3 long-turn handoff canary, canonical `6ab025a67d18819191ecd5408f42eb6f`.

Durable result: `T3_RESULT=FAIL`, reason `FINAL_CONFIRM_EXECUTED_BEFORE_SHIFTED_DUE`. The R Foreman explicitly rejected RRULE production adoption for that round because the final-confirm execution occurred before the shifted due, contaminating wake provenance.

Transferable lesson for Mer: a live schedule showing target T does not prove an invocation observed around T was caused by T. WAKE_OK requires provenance compatible with the intended target; early/manual/other dispatch must not be counted as target wake. This supports Mer's WF-S2 separation of STATE_OK from WAKE_OK.

Non-transferable: T3 does not establish same-canonical serialization or overlap. Its failure was provenance contamination, not a direct concurrency measurement.
