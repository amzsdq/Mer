# O1 Lease Calibration Design — 2026-09-24 04:46 KST

Status: SHADOW_IMMUTABLE_EVIDENCE
Primary variable: OWNER_LOSS_RECOVERY_MECHANISM

## Current blocker
The durable ownership record has no renewable liveness datum and still names generation 1 owner `MER-20260923T215700+0900-O1S001`. Therefore an expiry threshold cannot yet be calibrated from Mer owner behavior.

## Minimal candidate
Do not add a second heartbeat file. Extend the single authoritative `control/ownership.json` record only when a valid OWNER is available:
- `owner_renewal_seq`: monotonically increasing integer scoped to generation.
- Each renewal is a SHA-guarded update by the exact current owner+generation.
- Liveness time is the GitHub commit/server timestamp of the accepted ownership update, not a model-authored timestamp.
- A renewal may piggyback on a natural safe-unit/checkpoint boundary; dedicated heartbeat writes are admitted only if measured natural gaps are too wide for acceptable recovery latency.

## Calibration admission
Before choosing any lease duration, collect at least 5 consecutive genuine OWNER renewal intervals while useful work is occurring. Exclude synthetic rapid-write/API calibration bursts and SHADOW writes.

For samples g_i between accepted renewal commits:
- record n, min, median, max;
- separately record GitHub/API write latency when directly observable;
- do not infer scheduler dispatch latency from renewal gaps.

## Threshold rule
Do not import Kubernetes default seconds into Mer. Select the first Mer candidate only after genuine gaps exist. Candidate lease duration must exceed the largest normal observed renewal gap plus an explicit jitter/API safety margin. Then test the candidate rather than treating it as truth.

## Discriminating tests
1. FALSE_TAKEOVER_CANARY: healthy OWNER continues useful work and renewals; SHADOW must never acquire.
2. INTENTIONAL_OWNER_LOSS: OWNER stops renewing; after declared expiry, exactly one READY SHADOW may SHA-CAS generation+1.
3. DUPLICATE_CANDIDATE_RACE: two eligible SHADOWs race from the same fresh blob; exactly one update succeeds.
4. STALE_OWNER_FENCE: old OWNER attempts a subsequent authoritative side effect after takeover; fresh owner+generation check must reject it.
5. RECOVERY_LATENCY: measure from last accepted OWNER renewal server timestamp to successful takeover server timestamp.

## Promotion gate
Retain existing gate: 3 clean owner-loss recoveries, zero duplicate authoritative side effects, zero scheduler conflicts, plus comparator versus no-recovery baseline. Any false takeover under normal measured behavior => REVISE, not retry unchanged.

## External prior translation
Kubernetes Lease separates lease duration, renewal deadline, and retry period, and explicitly warns that leader election itself is not fencing. Mer therefore retains per-side-effect generation fencing and treats lease timing only as a liveness/recovery detector. etcd similarly ties election leadership to a lease and exposes a revision that can be tested for ownership; Mer's closest available primitive is generation plus optimistic-concurrency SHA on one authority record.

## Immediate next action
Acquire a valid OWNER through a safe recovery path, add `owner_renewal_seq` to the single authority record, then collect >=5 genuine renewal gaps before setting `expiry_threshold`.