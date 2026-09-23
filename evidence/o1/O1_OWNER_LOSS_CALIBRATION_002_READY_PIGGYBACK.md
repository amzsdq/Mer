# O1 OWNER LOSS CALIBRATION 002 — READY MARKER PIGGYBACK

Status: IMMUTABLE_SHADOW_ANALYSIS

The overlap protocol already requires a SHADOW successor to publish READY evidence. That READY commit can serve two roles without adding another control write:
1. prove successor preparation;
2. provide fresh GitHub-server observation time T for initial owner-liveness age evaluation.

If READY shows the owner lease is not yet expired, subsequent expiry evaluation should occur at a later meaningful boundary/wake rather than generating clock-only writes. If READY already shows expiry, candidate still must fresh-read ownership immediately before CAS.

This reduces recovery control overhead and aligns with the primary objective: reuse required evidence rather than introduce a dedicated time-probe mechanism.
