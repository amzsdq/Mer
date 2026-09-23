# O1 OWNER LOSS CALIBRATION 002 — FALSE TAKEOVER ORACLE

Status: PREPARED_NOT_AUTHORIZED

A false takeover cannot be judged from model narrative ('owner was alive'). The canary needs durable evidence.

Proposed oracle for healthy-owner canary:
- owner publishes valid server-timestamped liveness checkpoint for current generation according to fixed cadence;
- candidate observation marker T is server-timestamped;
- if T - latest valid renewal < declared P+G, any takeover attempt is mechanically premature/false;
- if candidate CAS occurs despite that relation, sample fails regardless of later success.

For a deliberately delayed owner beyond P+G, takeover may be detector-correct even if predecessor process is physically alive; safety must then be enforced by generation fencing. This distinction avoids conflating detector false-positive with stale-process existence.
