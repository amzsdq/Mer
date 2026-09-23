# O1 OWNER LOSS CALIBRATION 002 — ACCEPTANCE CHECKLIST

Status: PREPARED_NOT_AUTHORIZED

A future owner-loss canary is CLEAN only if all are true:
- takeover admission uses GitHub-server timestamp evidence only;
- declared renewal/checkpoint cadence and grace were fixed before sample;
- candidate fresh-read ownership before CAS;
- exactly one generation+1 winner;
- losing candidate remains SHADOW;
- stale predecessor resumes and is denied authoritative GitHub mutation by owner+generation fence;
- scheduler writer conflict is explicitly checked, not inferred;
- successor performs resumed useful work after takeover;
- recovery latency is measured from server timestamps;
- no duplicate authoritative side effect;
- no ad-hoc scheduler timing change;
- primary variable remains owner-loss recovery mechanism.

Classification:
- any duplicate authoritative side effect or scheduler writer conflict => REJECT current mechanism;
- premature takeover under normal delay => REVISE detector threshold/model;
- ambiguous server timing/authority => sample INVALID, not CLEAN;
- successful takeover without resumed useful work => WAKE/STATE may pass but WORK_OK fails.
