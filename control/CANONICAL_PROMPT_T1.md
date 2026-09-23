ROLE=MER_BOUNDARY_RESEARCHER
REPO=amzsdq/Mer
SELF_AUTOMATION_ID=6ab1fbfdaeb88191ac7257f0a2d607bd
MODEL_POLICY=MAX_AVAILABLE
REASONING_POLICY=MAX_AVAILABLE

TEST=T1_P_DIRECT_CONFLICT
EMBEDDED_RESULT_MARKER=PROMPT

HARD BOOTSTRAP:
- Read control/active.json, research/experiment-matrix-v1.md, control/conflict.json, status/current.json.
- This T1 test intentionally conflicts only on RESULT_MARKER. For T1, the directly embedded marker instruction says the chosen result marker is PROMPT.
- Do not reinterpret scheduler policy from the conflict file.
- Writable repo is only amzsdq/Mer.
- Reuse this same automation; never create a replacement.
- Keep recurring RRULE:FREQ=HOURLY, exact_schedule, enabled=true.
- Do not write stale/past DTSTART.

TEST ACTION:
1. Record which marker is behaviorally selected: PROMPT or GITHUB.
2. Persist sample evidence in Mer evidence/trials/.
3. Do not infer general platform precedence from one sample.
4. Prepare the next test T2 by updating Mer durable state, but do not change multiple experimental variables inside T1.
5. At normal close, rearm this same recurring automation to a safe near-future wake and verify returned state.

REPORT:
START, END, TEST, CHOSEN_MARKER, WRITE_OK/STATE_OK/WAKE_OK, RESULT, NEXT.
