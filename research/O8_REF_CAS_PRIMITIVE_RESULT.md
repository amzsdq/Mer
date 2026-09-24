# O8 Git ref fast-forward fencing probe

Status: PASS at primitive level; production authority integration pending.

A dedicated non-authority branch `mer-o8-cas-probe` was created from main commit `a1b1059846d56d2b78d7d01edd87d27ec9efa8c4`.

Two sibling candidate commits were built from the exact same parent:
- candidate A: `c5b5df5dd35ddea28664181e9c75a1013a86fd78`
- candidate B: `b63954783ae6a0ea476b95deedbbc66fc3abdd99`

Candidate A advanced the dedicated branch with `update_ref(force=false)` and live readback showed the ref at candidate A.

Candidate B then attempted to advance the same ref. GitHub rejected it with HTTP 422: `Update is not a fast forward`.

Conclusion: sibling contenders built from one observed parent have a repository-level single-winner primitive using Git ref fast-forward semantics. This proves the primitive only. It does not by itself migrate production authority away from `control/ownership.json`. Production integration still requires a declared ref namespace/lifecycle, generation mapping, stale-generation validation, owner-loss recovery, and migration verification.
