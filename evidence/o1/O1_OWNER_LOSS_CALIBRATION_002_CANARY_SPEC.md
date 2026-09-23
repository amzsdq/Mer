# O1 OWNER LOSS CALIBRATION 002 — CANARY SPEC

Status: PREPARED_NOT_AUTHORIZED
Primary variable: OWNER_LOSS_RECOVERY_MECHANISM

## Purpose
Prepare the next owner-admissible test without mutating authoritative state from SHADOW.

## Phase 1 — normal-renewal calibration
- Keep scheduler recurrence unchanged.
- Declare one renewal cadence P in authoritative execution state when an owner is available.
- Owner publishes valid immutable renewal markers carrying owner invocation id + generation.
- Measure GitHub server `created_at` intervals across real wakes.
- Do not perform takeover in this phase.

Pass: enough real samples to choose provisional grace G without mixing transport-only or recurrence-only observations.
Fail/Revise: renewal cadence cannot be maintained or server evidence is ambiguous.

## Phase 2 — false-takeover canary
- Keep owner alive and renewing normally.
- SHADOW evaluates expiry admission from fresh server evidence.
- Expected result: no takeover attempt before P+G.

Any premature admission => REVISE/REJECT threshold model.

## Phase 3 — intentional owner loss
- Owner intentionally omits renewal/normal transfer according to declared sample protocol.
- Candidate waits for server-observed P+G non-renewal.
- Fresh-read ownership; attempt exactly one SHA-guarded generation+1 CAS.
- Winner revalidates ownership before every authoritative side effect.

Pass requires exactly one winner, resumed useful work, no duplicate authoritative side effect, no scheduler writer conflict, bounded recovery latency.

## Promotion
Three clean owner-loss recoveries plus comparator; otherwise no promotion.

## Current authority note
Current durable ownership still names generation 1 predecessor. This file is preparation only; it grants no authority and performs no scheduler mutation.
