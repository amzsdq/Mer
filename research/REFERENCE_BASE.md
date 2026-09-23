# Reference Base

Status: SEED_REFERENCE_SET
Last refreshed: 2026-09-23

This file is not authority for Mer runtime behavior. It is a source of hypotheses, invariants, failure modes, and adverse-test ideas.

## Internal empirical sources
- amzsdq/tEST
  - relay lead-time tests
  - WRITE_OK / STATE_OK / WAKE_OK / WORK_OK separation
  - recurring RRULE self-shift / missed-wake / fallback work
- amzsdq/workwork
  - immediate prearm
  - runtime-boundary / productive-window / completion-envelope separation
  - close reserve estimation
  - overlap / shadow-successor handoff

## Authoritative implementation references

### Temporal
Reference: https://docs.temporal.io/
Relevant ideas:
- durable execution and resume-after-failure
- history-backed continuation
- worker/task-queue performance tuning
Translation candidates:
- durable state should be reconstructable independently of one transient invocation
- latency optimization must not erase recovery semantics

### Kubernetes Lease / leader election
References:
- https://kubernetes.io/docs/concepts/architecture/leases/
- https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/
Relevant ideas:
- one active holder
- renewable lease / expiry
- holder identity
- optimistic concurrency / version fencing
Translation candidates:
- overlap experiments require explicit single-authority fencing
- liveness and authority are separate concerns

### AWS Step Functions
Reference:
- https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html
Relevant ideas:
- explicit retry vs catch/fallback paths
- retry parameters are policy, not proof of success
- stage-aware recovery
Translation candidates:
- classify failure before retrying
- retries need bounded rules and alternate recovery paths

### Cloudflare Durable Objects Alarms
Reference:
- https://developers.cloudflare.com/durable-objects/api/alarms/
Relevant ideas:
- future wake as durable scheduled state
- at-least-once alarm delivery
- automatic retry with bounded backoff
- one current alarm per object, reschedule for next event
Translation candidates:
- scheduler acceptance and later execution are distinct evidence
- recurring/fallback wake paths must be tested under missed/failure conditions

## Academic references

### Chandra & Toueg (1996)
Unreliable failure detectors for reliable distributed systems.
Journal of the ACM 43:225-267.
Relevant ideas:
- failure detection is characterized by completeness and accuracy rather than perfect knowledge
- false suspicions and delayed detection are fundamental design concerns
Translation candidates:
- Mer watchdog/liveness logic should explicitly measure false-positive and missed-detection behavior
- timeout/overdue is evidence, not automatically proof of death

## Use rule
For each new hypothesis:
1. cite the relevant entries from this file or add new references;
2. state what property is transferable;
3. state what assumption is not transferable to ChatGPT Automations;
4. define the Mer experiment that would falsify the translated hypothesis.
