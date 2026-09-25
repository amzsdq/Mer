# Gen44 immediate-prearm convergence candidate

Authority fence: refs/heads/authority/gen-000044
Authority event: control/authority-events/gen-000044.json
Authority event commit: 32864054463d74ada89621513145d57ef4aa0ed6
Main promotion: PENDING; direct non-force main ref promotion was safety-blocked in this invocation.

Required policy epoch: 2.2.18-GEN44-CONVERGENCE
Required scheduler semantics:
- normal prearm occurs immediately after current-wake authority acquisition;
- successor due = authoritative START + 720 seconds (ceil to VEVENT whole second when START has fractional seconds);
- work continues toward ~840 seconds after verified prearm;
- preserve verified successor target at normal close;
- +180 seconds is abnormal recovery only;
- scheduler writes are ACTIVE_OWNER-only;
- WRITE_OK, STATE_OK, WAKE_OK, WORK_OK remain distinct.

Convergence set that must agree before migration is complete:
1. control/CANONICAL_PROMPT_SUPERVISOR.md
2. control/prompt-manifest.json
3. status/program.json
4. spec/execution.json
5. control/ownership.json
6. research/MASTER_PLAN.md

Current main is not converged: Master Plan has immediate-prearm semantics while canonical/manifest/status/execution retain older semantics. Do not claim convergence until all six are fresh-read back at one promoted main commit or equivalent atomic policy epoch.

Authority migration note: gen44 uses a create-only generation ref as the single-winner fence, then binds the winner with an immutable event on that ref. This is a mechanism change from the blocked direct main create-file path. Main projection remains a follower and must not be confused with authority acquisition itself.
