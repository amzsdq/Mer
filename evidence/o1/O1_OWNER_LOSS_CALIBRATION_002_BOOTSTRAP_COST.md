# O1 OWNER LOSS CALIBRATION 002 — BOOTSTRAP COST OBSERVATION

Status: IMMUTABLE_SHADOW_ANALYSIS

Recovery latency includes candidate bootstrap reads: prompt manifest, active pointer, program state, execution state, and ownership. These reads are necessary for authority reconstruction but should remain minimal.

For an already-awake SHADOW successor, most research preparation can occur before takeover; after takeover only fresh ownership/scheduler verification and authoritative state reconciliation should remain. This is another reason controlled overlap can reduce resume latency independently of lease threshold.

Future WORK_OK measurements should distinguish pre-takeover SHADOW preparation from post-takeover authoritative resume so overlap benefit is visible rather than credited to CAS itself.
