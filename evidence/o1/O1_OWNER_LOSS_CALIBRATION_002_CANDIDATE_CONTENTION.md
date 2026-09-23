# O1 OWNER LOSS CALIBRATION 002 — CANDIDATE CONTENTION

Status: IMMUTABLE_SHADOW_ANALYSIS

If multiple SHADOW successors are possible, no separate election protocol is required before takeover if all contenders use the same ownership SHA/CAS and generation+1 target semantics. GitHub conflict selects the ownership-record winner; losers fresh-read and remain SHADOW.

Do not add a second lock file solely to serialize takeover candidates unless experiments show Contents API conflict handling is insufficient. A second lock would add another authority domain and failure mode.

This is a simplicity conclusion: reuse the ownership record CAS as the contention point.
