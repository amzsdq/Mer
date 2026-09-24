# O1 Recovery Analysis

Current renewal calibration cannot progress under current authority: execution requires at least five genuine OWNER renewal gaps, while ownership still names generation 2 with renewal sequence 1 and later wakes are SHADOW.

Decision: REVISE. Do not count SHADOW scheduler wakes as OWNER renewals. The next useful experiment should explicitly authorize one bounded recovery transition for the stuck generation, then resume genuine OWNER liveness measurement from the recovered owner. This wake does not change program, execution, or ownership authority.

External comparison: Kubernetes configures lease duration, renew deadline, and retry period as coordination parameters and uses optimistic concurrency for acquisition; etcd election leadership is attached to a lease. These references support separating one-time recovery from later steady-state liveness calibration, but they are not treated as Mer validation.
