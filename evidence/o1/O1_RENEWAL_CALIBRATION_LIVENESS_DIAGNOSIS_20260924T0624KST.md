# O1 renewal calibration liveness diagnosis

## Observation
- Authoritative ownership remains generation 2, active_invocation_id MER-20260924T060808+0900-O1BOOT001.
- GitHub commit history for control/ownership.json shows bootstrap takeover at 2026-09-23T21:08:31Z and exactly one renewal at 2026-09-23T21:08:38Z.
- The current wake is a different invocation and therefore is SHADOW under the generation/active_invocation fence.

## Diagnosis
The current calibration plan cannot naturally collect >=5 genuine renewal gaps across wakes while ownership identity is invocation-scoped. Once the generation-2 bootstrap invocation ends, later wakes cannot renew its lease without first transferring/recovering ownership. Treating later SHADOW writes as owner renewals would violate the fence and contaminate calibration.

## Model update
REVISE. Do not infer an expiry threshold from the 7-second bootstrap bookkeeping gap. Before lease calibration continues, define and test a normal successor ownership-transfer path that changes active_invocation_id/generation at handoff, or explicitly redefine the liveness principal from ephemeral invocation to a durable logical worker identity and test fencing implications. Prefer normal handoff because it preserves the existing invocation-scoped fencing model and changes fewer assumptions.

## Next experiment
Test one clean normal handoff: successor AWAKE -> REARMED -> READY, predecessor-mediated CAS transfer with generation increment, then successor ACTIVE. Measure server timestamps for readiness, transfer, first useful owner side effect, and later successor wake behavior. Only after repeated live owner intervals exist should lease expiry calibration resume.
