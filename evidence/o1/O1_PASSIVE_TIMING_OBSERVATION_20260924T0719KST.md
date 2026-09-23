# O1 Passive Timing Observation — 2026-09-24 07:19 KST

## Purpose
Test whether existing Mer durable evidence is sufficient to derive a provisional time-only owner-expiry threshold without inventing a lease value.

## Observation
- Deployed prompt manifest matches PROMPT_VERSION=2.2.6-WORKAHOLIC-HANDOFF / PROMPT_ID=MER-OPT-2I.
- status/program.json still points to O1 owner-loss recovery / expiry calibration.
- spec/execution.json still requires at least 5 genuine owner renewal gaps and leaves expiry_threshold=UNSET_REQUIRES_CALIBRATION.
- control/ownership.json still names generation 2 owner MER-20260924T060808+0900-O1BOOT001 with owner_renewal_seq=1.
- Repository search for actual wake / READY observations returned no indexed comparable samples.

## Result
INSUFFICIENT_TIMING_EVIDENCE remains the supported conclusion. Do not derive expiry from the +840s scheduler pre-arm, because scheduler lead and failure-detector timeout are distinct variables. Do not use the bootstrap bookkeeping gap as a representative renewal interval.

## Hypothesis update
KEEP passive timing observation as the next evidence-gathering mechanism; REJECT promotion of any provisional time-only expiry until comparable actual wake/READY or liveness observations exist. If repeated passive samples cannot separate healthy-owner activity from wake/READY delay, REJECT time-only expiry and test an explicit renewable liveness signal instead.

## Authority
This file is immutable evidence only. It does not change status/program.json, spec/execution.json, or control/ownership.json.