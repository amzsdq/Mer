# H-O6-OBSERVED-STATE-RECONCILIATION

HYPOTHESIS_ID: H-O6-OBSERVED-STATE-RECONCILIATION
STATUS: TESTABLE

CLAIM: For the remaining O6 fault classes, Mer should treat scheduler/prompt/bootstrap operations as desired-state writes that require later observed-state reconciliation, rather than treating an accepted write or configured artifact as proof of execution. A bounded reconcile-on-wake path should recover prompt mismatch and missing bootstrap from stable recovery references, and should classify scheduler WRITE_OK without later WAKE_OK as missing observed execution while preserving the hourly recurring fallback and never granting substantive authority from scheduler state alone.

PRIMARY_VARIABLE: RECONCILIATION_MODE
- A: ACK_BASED — accepted write/config presence is treated as sufficient success evidence.
- B: OBSERVED_STATE — success requires independent observed state: prompt manifest/live prompt match for sync, reconstructable bootstrap state for authority, and actual later wake evidence for scheduler delivery.

SOURCE_CLASS:
- INTERNAL_EMPIRICAL: Mer distinguishes WRITE_OK/STATE_OK/WAKE_OK/WORK_OK; gen24 incident showed configured continuation can coexist with stranded substantive authority.
- AUTHORITATIVE_IMPLEMENTATION: Kubernetes controllers continuously reconcile actual state toward desired state; object spec is desired state and status is observed current state. GitHub Actions documents that scheduled events can be delayed and, under sufficiently high load, queued jobs can be dropped.

SUPPORTING_PRIORS:
1. Kubernetes Controllers: a controller is a non-terminating control loop that observes current state and makes changes toward desired state. This supports separating desired configuration from observed execution.
   https://kubernetes.io/docs/concepts/architecture/controller/
2. Kubernetes Objects: spec describes desired state while status describes current state, maintained by the control plane.
   https://kubernetes.io/docs/concepts/overview/working-with-objects/
3. GitHub Actions schedule documentation: scheduled events can be delayed, and under high load queued jobs may be dropped. This is not evidence about ChatGPT Automations specifically; it is authoritative evidence that an accepted schedule/configuration in a mature scheduler need not imply timely later execution.
   https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule

COUNTER_PRIORS / KNOWN_CONFLICTS:
- Continuous reconciliation can add control overhead and can loop forever when desired state is impossible. Therefore B must be bounded and evidence-driven; it must not blindly rewrite already-correct state.
- Provider-specific GitHub Actions behavior is non-transferable to ChatGPT Automations. Mer must empirically test WRITE_OK->WAKE_OK separation rather than assume dropped wakes occur here.
- Missing control/active.json is not automatically corruption because the existing bootstrap contract explicitly permits stable recovery references.

TRANSLATION:
- Desired scheduler state = same automation enabled + exact_schedule + recurring hourly VEVENT + intended DTSTART.
- Observed scheduler state = live readback immediately after write; later execution is separately WAKE_OK.
- Desired prompt state = manifest desired version/id; observed prompt state = deployed automation prompt version/id.
- Bootstrap recovery = reconstruct from spec/GOAL.md + research/MASTER_PLAN.md + status/program.json when control/active.json is missing/unreadable; no authority mutation until ownership is validated.

NON_TRANSFERABLE_ASSUMPTIONS:
- Kubernetes watch/cache semantics do not exist here by default.
- GitHub Actions cron timing/drop behavior does not establish ChatGPT Automation timing/drop behavior.
- Mer uses GitHub durable files and explicit reads rather than an API-server controller cache.

DISCRIMINATING_TEST:
R1 prompt mismatch fixture: desired manifest version differs from deployed version. A would accept stale deployment/config; B must detect mismatch, select canonical prompt as desired state, and require deployed live match before substantive work.
R2 missing bootstrap fixture: control/active.json unavailable. B must reconstruct only from declared stable recovery references, validate ownership separately, and avoid invented state.
R3 scheduler ACK-without-WAKE fixture: WRITE_OK and immediate STATE_OK exist but no later WAKE_OK. B must classify delivery as unproven/missing, preserve recurring fallback, and must not infer WORK_OK or substantive ownership.
R4 matched nominal fixture: all desired/observed states already match. B must perform no unnecessary corrective rewrite beyond the one allowed wake-start prearm, bounding overhead.

PROMOTION_GATE:
All four tests pass with zero duplicate authoritative side effects, zero scheduler disablement, no false WRITE_OK=>WAKE_OK/WORK_OK inference, correct prompt/bootstrap recovery, and no extra corrective mutation in nominal matched state.

REJECTION / REVISION RULE:
If observed-state reconciliation adds corrective writes in the nominal matched case, creates authority from scheduler evidence, or cannot distinguish missing wake from delayed wake without unsafe inference, REVISE. If ACK_BASED performs equivalently on the adverse cases with lower control cost, REJECT B under the simplicity objective.
