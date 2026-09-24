# H-O8-SERIALIZED-SAME-CANONICAL

Status: TESTABLE — REPAIR CANDIDATE AFTER B03 DISPATCH FAILURE

## Claim
Under current conditions, the same canonical does not reliably dispatch a concurrent successor while its prior invocation remains active. Normal continuation should therefore be tested as serialized: sustain useful work, then at legal close rearm the SAME canonical from an externally timestamped PRE_CLOSE by a short fixed delay, verify exact live state, persist END, and let the next invocation resume from durable state.

## Triggering Mer evidence
B03/gen37 target was `2026-09-24T15:38:39Z`. Gen37 START is GitHub `15:24:20Z`; a GitHub marker proves gen37 remained active at `15:38:39Z`; posttarget observation at `15:39:09Z` found canonical last_run_time still `15:20:54.514328Z` and no distinct gen38 START/READY. Classification: `SERIALIZED_OR_DEFERRED_CANDIDATE_SAMPLE1`. One sample is not a universal provider contract.

## Prior correction
The former workwork 198s overlap proof is invalidated by GitHub chronology: primary START commit 16:17:32Z, primary END commit 16:18:57Z, observer commit 16:29:46Z. Model-written clocks created the old apparent overlap. See corrected `research/WORKWORK_OVERLAP_INTAKE.md`.

## Related prior
RRuleRO #72 comment 5816667224 independently records `NO_CONCURRENT_SUCCESSOR_EVIDENCE` after a same-canonical due while owner remained active. Treat as related prior only. R issue #504 T3 shows wake-provenance contamination risk: an execution before intended due cannot prove target wake.

## Authoritative-reference gap
A bounded OpenAI Help Center search found recurring-task lifecycle/cadence documentation but no explicit same-task concurrency/serialization contract. Absence of documentation is not proof.

## Competing explanation
Scheduler jitter/delayed dispatch could explain one missed concurrent target. Require repeated Mer samples and actual next-wake measurement.

## Declared compound repair test
Because overlap-dependent prearm failed its intended boundary, first serialized repair intentionally changes a coupled mechanism bundle: no concurrent-successor expectation; current owner works until close; GitHub PRE_CLOSE server timestamp; same canonical PRE_CLOSE+120s; complete recurring VEVENT; exact live readback; END only after verified continuation; next invocation must prove WAKE_OK and WORK_OK. Hold +120s fixed within serialized samples; optimize delay later.

## Pass gate
Each clean serialized sample requires protocol-complete pre-mutation intent, exact WRITE_OK+STATE_OK, END after verified continuation, next distinct START after END, compatible WAKE_OK provenance, WORK_OK, no duplicate authority, and measured idle gap. Require >=3 clean serialized samples before promotion plus owner-death-before-close recovery evidence.

## Reject/revise
Reject if close-relative rearm repeatedly misses continuation or materially worsens idle/recovery versus a validated alternative. If provider occasionally overlaps, retain generation fencing for substantive authority while keeping scheduler writes owner-only.
