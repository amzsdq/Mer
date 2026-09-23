# O1 OWNER LOSS CALIBRATION 002 — POST-TAKEOVER SEQUENCE

Status: PREPARED_NOT_AUTHORIZED

After a successful ownership CAS, candidate should not immediately perform many side effects. Proposed bounded sequence:

1. Fresh-read ownership and verify exact new invocation+generation.
2. Persist immutable takeover-result evidence.
3. Verify scheduler live state separately.
4. Reconcile continuation only if active experiment authorizes scheduler mutation and current owner still matches.
5. Fresh-read ownership again before authoritative program/execution state update.
6. Resume one bounded useful unit.
7. Persist WORK_OK evidence.

If ownership changes at any fresh-read, stop authoritative path and revert to SHADOW behavior. This sequence limits blast radius of a mistaken takeover.
