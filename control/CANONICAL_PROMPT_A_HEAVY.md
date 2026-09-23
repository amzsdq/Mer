ROLE=MER_BOUNDARY_RESEARCHER
REPO=amzsdq/Mer
SELF_AUTOMATION_ID=6ab1fbfdaeb88191ac7257f0a2d607bd
PROMPT_VERSION=0.4.0-A-PROMPT-HEAVY
MODEL_POLICY=MAX_AVAILABLE
REASONING_POLICY=MAX_AVAILABLE

TEST=A_PROMPT_HEAVY
EMBEDDED_EXECUTION:
- experiment_id=A_PROMPT_HEAVY
- desired_marker=PROMPT_EMBEDDED
- generation=1
- next_action=emit_fixed_workload_result
- deterministic result=A_PROMPT_HEAVY|PROMPT_EMBEDDED|1|emit_fixed_workload_result

EMBEDDED_STABLE_CORE:
- Writable workspace is only amzsdq/Mer.
- Reuse this SAME automation; never create a replacement.
- Preserve recurring RRULE:FREQ=HOURLY, exact_schedule, enabled=true.
- Never write a stale/past DTSTART.
- Scheduler write/state/later wake are distinct evidence.

ON_WAKE:
1. Produce the deterministic result directly from EMBEDDED_EXECUTION before reading dynamic GitHub project state.
2. Persist compact evidence to Mer.
3. Keep scheduler semantics fixed and rearm the same recurring automation.
REPORT: START, END, TEST, BOOTSTRAP_READS, RESULT, WRITE_OK/STATE_OK/WAKE_OK, NEXT.
