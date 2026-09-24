# O8 Rejected / Revised Mechanisms

Status: ACTIVE LEDGER

## ACQUISITION_IMPLIES_INVOCATION_EXIT
Disposition: REJECT
Reason: it conflates a role transition with invocation termination. Gen34 directly demonstrated that an invocation can acquire generation ownership and continue through multiple distinct useful units without duplicate substantive authority.
Replacement: ROLE_RELATIVE_STOP_GATE.

## BLIND_EARLY_EXIT_PLUS_180S_RERUN
Disposition: REJECT AS NORMAL RECOVERY
Reason: repeated short nonterminal invocation -> +180s -> same unchanged path reduces idle gap but does not repair why the invocation stopped early. It can mask the primary long-wake failure.
Replacement: EARLY_EXIT_REPAIR_FIRST in the same invocation; +180s remains fallback only when same-turn continuation/repair is genuinely unavailable.

## WRITE_OK_EQUALS_SCHEDULER_SUCCESS
Disposition: REJECT
Reason: scheduler update acknowledgement, live state, later wake, and resumed work are distinct evidence states.
Replacement: WRITE_OK / STATE_OK / WAKE_OK / WORK_OK tracked separately.

## FUTURE_DTSTART_ONLY_LIVE_VERIFY
Disposition: REJECT
Reason: a future DTSTART can be the wrong instant. The incident requires exact intended instant plus recurrence/timing/enabled/ID verification.
Replacement: exact full-state live readback.

## UNATTRIBUTED_SCHEDULER_MUTATION_DIAGNOSIS
Disposition: REVISE
Reason: automation metadata does not expose writer identity. Gen34 proved the canonical target can be overwritten within seconds, but timing alone cannot identify the writer.
Replacement: immutable SCHEDULER_WRITE_INTENT + SCHEDULER_WRITE_RESULT attribution protocol.

## OFFSET_OR_TIMEZONE_AS_UNIQUE_ROOT_CAUSE
Disposition: NOT PROVEN / DO NOT PROMOTE
Reason: the historical 23:45:17 -> 07:30 mismatch lacked preserved raw write/readback evidence, and gen34 separately proved a concurrent last-writer overwrite surface. Representation remains one candidate, not the unique established cause.
Next discriminator: full-VEVENT absolute representation held constant through later WAKE_OK/WORK_OK, then scheduler-writer fencing as a separate variable if warranted.

## ALL_WAKES_PREARM
Disposition: ACTIVE BASELINE, UNDER CHALLENGE
Reason: it provides crash insurance but exposes one canonical scheduler to concurrent last-writer mutation. Gen34 observed 15:01:31Z -> 14:51:15Z overwrite within ~4.7s while substantive ownership remained gen34.
Candidate alternative: H-O8-SCHEDULER-WRITER-FENCE. Do not promote until continuity/recovery adverse testing passes.
