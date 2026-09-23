# O1 OWNER LOSS CALIBRATION 002 — COMPACT RECOVERY PROTOCOL CANDIDATE

Status: IMMUTABLE_SHADOW_SYNTHESIS

## Healthy path
1. Current owner remains sole scheduler writer.
2. Owner prearms successor per active overlap strategy.
3. Successor wakes SHADOW and prepares.
4. Owner-mediated generation transfer remains preferred; no lease wait on healthy path.

## Recovery path only
1. SHADOW observes latest valid server-timestamped liveness evidence for current owner+generation.
2. If evidence age has not crossed calibrated admission threshold, remain SHADOW and continue non-authoritative useful work.
3. After threshold, fresh-read ownership and attempt one SHA/CAS generation+1 takeover.
4. CAS loser remains SHADOW.
5. Winner revalidates owner+generation before authoritative mutations.
6. Verify scheduler live state separately; do not infer scheduler safety from GitHub CAS.

## Liveness evidence preference
First preference: piggyback already-required bounded checkpoints if real OWNER measurements show usable cadence.
Fallback: dedicated renewal markers if checkpoint cadence is too sparse/unbounded.

## Non-goals
- lease does not replace fencing;
- recovery path does not replace healthy owner-mediated handoff;
- no threshold selected from model clocks, arbitrary write latency, or raw hourly recurrence.

This is the smallest coherent mechanism currently supported by Mer evidence. It remains unpromoted until canary gates pass.
