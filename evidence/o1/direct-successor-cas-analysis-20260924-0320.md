# Direct successor CAS analysis

Status: CANDIDATE, NOT PROMOTED

Observed state: durable ownership is still generation 1 and names the old 21:57 invocation. The current baton runtime wakes successors independently via a one-write +840s scheduler lane.

## Competing mechanisms
A. LEASE_EXPIRY: successor waits for server-timestamp non-renewal, then CAS generation+1.
B. DIRECT_SUCCESSOR_CAS: an actual scheduled successor with durable WAKE/READY evidence attempts exactly one fresh-read CAS generation+1 immediately; stale predecessor is fenced at every later authoritative side effect by exact owner+generation check.

## Why B may be simpler
- no heartbeat field
- no expiry threshold or clock-delay calibration
- no voluntary idle while successor waits for a dead owner
- reuses the existing generation CAS fence

## Safety conditions for B
1. Candidate identity must be tied to an actual intended successor wake, not arbitrary SHADOW self-promotion.
2. Ownership update must fresh-read the current blob and increment generation exactly once using optimistic concurrency.
3. Every predecessor authoritative side effect must fresh-read exact owner+generation immediately before mutation.
4. Scheduler pre-arm remains separate continuity plumbing and must not imply substantive ownership.
5. Competing successor race must produce exactly one CAS winner; loser remains SHADOW.

## Discriminating tests
T1 live predecessor: successor CAS wins; predecessor next authoritative write is fenced; no duplicate side effect.
T2 dead predecessor: successor CAS wins without expiry wait and resumes useful work.
T3 two competing successors: exactly one wins generation CAS; zero duplicate authoritative writes.
T4 stale successor/replay: candidate not tied to current intended wake must be rejected.

## Decision rule
Promote DIRECT_SUCCESSOR_CAS over LEASE_EXPIRY only if T1-T4 are clean and recovery is no less safe while eliminating lease/heartbeat complexity. Otherwise REVISE or retain lease candidate.
