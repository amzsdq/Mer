# O1 OWNER LOSS CALIBRATION 002 — STALE OWNER RACE ANALYSIS

Status: IMMUTABLE_SHADOW_ANALYSIS

## Race
A candidate may observe lease expiry while predecessor is merely delayed. Candidate CAS advances generation. Predecessor then resumes.

## Required outcome
The resumed predecessor must become harmless without relying on it noticing a timeout. Before each authoritative shared-state or scheduler mutation it fresh-reads ownership; generation mismatch denies the side effect.

## Residual race
Fresh-read then side-effect is not an atomic transaction across arbitrary files/scheduler. A generation can change after the read but before a separate side effect. Therefore the current 'fresh-check immediately before' rule reduces but does not mathematically eliminate TOCTOU for side effects that cannot carry the generation CAS in the same transaction.

## Consequence
For GitHub state, authoritative mutable writes should where possible be made against the same SHA/versioned authority or include generation in the target record and use optimistic concurrency. Scheduler mutation lacks a GitHub transaction and remains a higher-risk external side effect.

## Experimental requirement
Owner-loss canary must explicitly include a stale-owner-resumes step and attempt a fenced authoritative action after takeover. If the stale owner can mutate scheduler or shared state despite generation change, REJECT the mechanism or strengthen fencing before promotion.
