# F5 workload — prompt drift detection

Primary variable versus F4: prompt desired/deployed version equality is intentionally false while the currently deployed prompt remains the known-good 0.3.0-B-HYBRID.

Procedure:
1. Read control/prompt-manifest.json.
2. Compare desired_version and deployed_version plus their canonical blob identities.
3. Treat PREPARED with desired!=deployed as detectable PROMPT_DRIFT_PREPARED, not as ACTIVE convergence.
4. Verify previous_good_version exists and equals the still-deployed known-good version.
5. Do not deploy the candidate during this detection sample; preserve scheduler semantics.
6. Persist compact evidence.

Expected: drift is detected without confusing a prepared candidate with the active deployed prompt; rollback metadata remains sufficient to identify the previous known-good version.
