# O8 Writer-Fence Fencing-Token Note — 2026-09-24

Status: SOURCING / SAFETY ADDENDUM

Martin Kleppmann's distributed-lock analysis highlights a failure mode directly relevant to Mer: a process can pause beyond a lease, later resume, and still attempt a stale side effect. A lock/lease by itself is insufficient when correctness matters; the protected resource must reject stale actors using a monotonically increasing fencing token.

Source: https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html

## Mer mapping
Mer's monotonically increasing `generation` is already intended to act as a fencing token for authoritative shared-state writes. O8 must preserve that property while changing scheduler-writer authority. The B-arm must therefore never weaken generation checks merely to recover scheduler liveness.

## Consequence for owner-loss adverse test
A stale genG predecessor that resumes after genG+1 recovery must be unable to publish authoritative shared-state effects. This is exactly WF-N6/WF-A3 territory. Recovery PASS requires both liveness (new owner resumes) and fencing safety (old generation rejected/fails closed).

## Scheduler-specific caveat
The automation scheduler itself does not expose a native generation/fencing-token parameter in its update surface. Therefore Mer cannot assume the scheduler provider will reject a stale writer. Writer authority must be enforced before calling scheduler mutation, and immutable attribution must detect any uninstrumented overwrite. If owner-loss recovery requires independently recoverable scheduler authority, a separate durable scheduler-writer epoch is the safer REVISE candidate than allowing unfenced all-SHADOW writes.

## Decision impact
No promotion yet. This source strengthens the hard requirement that B must demonstrate stale-owner fencing plus recovery, not just fewer scheduler overwrites.
