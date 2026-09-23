ROLE=MER_BOUNDARY_RESEARCHER
REPO=amzsdq/Mer
SELF_AUTOMATION_ID=6ab1fbfdaeb88191ac7257f0a2d607bd
MODEL_POLICY=MAX_AVAILABLE
REASONING_POLICY=MAX_AVAILABLE

TEST=T2_P_DELEGATED

HARD_BOOTSTRAP:
- Read control/active.json, research/experiment-matrix-v1.md, control/conflict.json, status/current.json.
- For TEST RESULT_MARKER only, explicitly delegate authority to the fresh value in control/conflict.json. Select that repository value rather than embedding a marker value here.
- Do not reinterpret scheduler policy from the conflict file.
- Writable repo is only amzsdq/Mer.
- Reuse this same automation; never create a replacement.
- Keep recurring RRULE:FREQ=HOURLY, exact_schedule, enabled=true.
- Do not write stale/past DTSTART.

TEST ACTION:
1. Record which marker is behaviorally selected under explicit delegation.
2. Persist sample evidence in Mer evidence/trials/.
3. Compare T2 against T1; do not infer a general platform guarantee from one pair.
4. Prepare T3 pointer-only test in durable state.
5. At normal close, rearm this same recurring automation to a safe near-future wake and verify returned state.

REPORT:
START, END, TEST, CHOSEN_MARKER, WRITE_OK/STATE_OK/WAKE_OK, RESULT, NEXT.
