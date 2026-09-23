# O1 Successor Admissibility Review — 2026-09-24 08:00 KST

Status: IMMUTABLE_SHADOW_DIAGNOSIS
Authority: none; does not mutate status/program.json, spec/execution.json, or control/ownership.json.

## Question
Can the promoted DIRECT READY-successor CAS candidate be made admissible using only current durable state and the current same-automation baton, without adding an unvalidated time lease or an unsafe broad self-promotion rule?

## Fresh authoritative state
- status/program.json remains O1_OWNER_LOSS_RECOVERY_TEST_DESIGN_AND_EXPIRY_CALIBRATION.
- spec/execution.json still requires NORMAL_OWNER_MEDIATED_OR_EXPERIMENTAL_EXPIRED_OWNER_CAS_AFTER_CALIBRATION and has no bootstrap exception remaining.
- control/ownership.json generation=2 remains bound to terminated bootstrap invocation MER-20260924T060808+0900-O1BOOT001; intended_successor_invocation_id is only NEXT_WAKE_SHADOW_PENDING_ID.

## Prior evidence reconciled
O1_DIRECT_SUCCESSOR_CAS_TEST_PROTOCOL_TURN4 correctly shows that SHA-guarded generation CAS plus fresh fencing can serialize a transfer, but it also states that a legitimate non-successor wake must not be allowed to seize ownership.

O1_DIRECT_SUCCESSOR_IDENTITY_DESIGN_20260924T0347KST already identifies the minimal missing primitive: predecessor-minted single-use successor identity/token bound to the current generation. Its decisive operational gate is whether the baton can actually deliver that token to exactly one next invocation.

The 07:47 synthesis proposed a weaker alternative: an experimentally declared one-shot recovery admission rule. That is useful only as a canary exception; it is not an ordinary handoff identity primitive and repeating it each wake would collapse into broad self-promotion.

## Finding
Current state does NOT contain enough information to prove that this wake is the uniquely intended successor. The placeholder NEXT_WAKE_SHADOW_PENDING_ID is not an identity. Same automation identity, scheduler wake order, +14m proximity, READY evidence, and successful pre-arm prove readiness/liveness properties, not successor identity.

Therefore DIRECT READY-successor CAS is NOT yet admissible as an ordinary ownership-transfer mechanism. Executing it now would violate the existing no-ad-hoc-self-promotion rule and would test two variables at once: transfer trigger plus an implicit successor-identity rule.

## Minimal next discriminating experiment
Do not mutate ownership. Test only SUCCESSOR_TOKEN_DELIVERY_FEASIBILITY as a non-authoritative primitive:
1. predecessor-side test fixture durably mints opaque token T for generation G and marks it ARMED;
2. baton mechanism must deliver T to exactly one intended next invocation without editing the stable deployed prompt and without using scheduler time as identity;
3. intended invocation writes immutable receipt containing hash(T), generation G, and its invocation id;
4. a second/duplicate invocation must not possess a valid T;
5. no ownership transfer occurs in this feasibility test.

If the platform cannot carry per-baton data into exactly one next invocation, REJECT tokenized direct CAS operationally and return to an explicit failure-detector/lease recovery design. If it can, then test T1-T5 from the identity design before promotion.

## Model update
- Broad READY-successor CAS: REJECT for now; insufficient admissibility.
- One-shot recovery admission: KEEP only as bounded bootstrap/canary mechanism, not ordinary handoff.
- Single-use successor token: KEEP as the minimal direct-CAS admissibility candidate.
- Immediate next primary variable: SUCCESSOR_TOKEN_DELIVERY_FEASIBILITY.

## Safety
No authoritative state changed. Generation 2 remains authoritative until a separately authorized transition is declared by durable program/execution state.