# O1 Successor Token Delivery Feasibility — 2026-09-24 08:14 KST

Status: IMMUTABLE_SHADOW_EVIDENCE
Authority: none. This record does not mutate status/program.json, spec/execution.json, or control/ownership.json.
Primary variable: SUCCESSOR_TOKEN_DELIVERY_FEASIBILITY

## Test question
Can the current same-automation +14m baton deliver a predecessor-minted opaque token T to exactly one intended next invocation, without changing the stable deployed prompt, using scheduler time as identity, or adding another coordination channel?

## Current primitive inventory
1. The scheduler mutation interface for this automation accepts title/prompt/schedule/timing/enabled state. A wake-start pre-arm changes DTSTART while preserving the same recurring automation and deployed prompt.
2. The deployed prompt is intentionally stable. Per-baton mutable state is required to live in GitHub rather than being injected by prompt mutation.
3. GitHub durable state can store an ARMED token, but a future invocation reading that shared record would learn the same token as any duplicate/concurrent invocation with repository read access.
4. control/ownership.json currently has only intended_successor_invocation_id=NEXT_WAKE_SHADOW_PENDING_ID, not a unique invocation binding.
5. No current baton primitive exposes a per-invocation private payload or a scheduler-returned future invocation identifier that the predecessor can bind before wake.

## Attempted construction
Candidate A — store plaintext T in GitHub and have next wake read it.
REJECT: shared-readable token is bearer capability available to every concurrent/duplicate wake; it proves possession of shared state, not intended-successor identity.

Candidate B — store hash(T) in GitHub and derive/present T from scheduler time or automation identity.
REJECT: either T is derivable by every wake from shared inputs or time becomes identity, both violating the discriminating test.

Candidate C — mutate deployed automation prompt to carry T to the next wake.
REJECT under current architecture: normal continuation reuses the same automation and the deployed prompt is the stable kernel; a prompt mutation is not scoped to exactly one future invocation and risks leaking T to later/overlapping wakes. It also consumes scheduler/prompt mutation semantics rather than testing the existing baton.

Candidate D — create a separate per-successor automation/message/channel containing T.
REJECT for this candidate: that adds a new coordination/delivery channel and violates the simpler same-automation mechanism being tested.

## Result
SUCCESSOR_TOKEN_DELIVERY_FEASIBILITY = FAIL under the currently available baton primitives.

The failure is operational, not cryptographic. GitHub can durably hold a token and SHA-CAS can serialize consumption, but the current runtime lacks a way for the predecessor to bind a secret/capability to exactly one not-yet-known future invocation. A shared ARMED token plus atomic consumption can elect exactly one winner, but it cannot prove that the winner is the intended successor; that is equivalent to broad first-ready CAS with an extra token field.

## Model update
- Tokenized DIRECT_SUCCESSOR_CAS as an ordinary handoff identity mechanism: REJECT under current primitives.
- Shared token as race serialization only: redundant with GitHub SHA CAS; do not add it.
- DIRECT_SUCCESSOR_CAS may be reconsidered only if the platform later exposes a per-wake private payload/future invocation identity, or if Mer explicitly changes the admissibility model from intended-successor identity to first-valid-ready contender.
- Immediate research path returns to explicit owner-loss failure detection/recovery. However, the existing same-owner renewal calibration remains structurally impossible while ownership is invocation-scoped and the recorded owner has terminated. The next discriminating design question is therefore not another passive timing sample: test whether a durable logical owner identity/renewable lease can span invocations without weakening generation fencing, or redesign owner-loss recovery around an explicitly authorized contender model.

## Safety
No ownership transfer occurred. No authoritative mutable state changed. Generation 2 remains authoritative under the current execution spec.