# Stage 4C — C_POINTER_ONLY repeated dynamic-state churn

Time: 2026-09-23 20:02 KST
Candidate: C_POINTER_ONLY / 0.5.0-C-POINTER

Two sequential GitHub-only mutations were applied while the deployed prompt remained unchanged.

- sample 1: generation 2 -> 3; marker GITHUB_DYNAMIC_MUTATED -> GITHUB_DYNAMIC_CHURN_1; readback PASS.
- sample 2: generation 3 -> 4; marker GITHUB_DYNAMIC_CHURN_1 -> GITHUB_DYNAMIC_CHURN_2; readback PASS.

Prompt mutations required: 0.
Dynamic split-brain observed: 0 for generation/marker, because those values are not duplicated in the pointer prompt.
Bootstrap model remains pointer -> repository; normal-path read cost remains higher than prompt-heavy but mutation cost is confined to GitHub.

Gate: 2/2 PASS.
