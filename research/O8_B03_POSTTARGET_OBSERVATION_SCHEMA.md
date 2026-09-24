# O8 B03 Post-Target Observation Schema

After 15:38:39Z, capture without mutating scheduler first:
- observation GitHub server timestamp;
- live canonical ID/enabled/timing/RRULE/DTSTART;
- live last_run_time;
- whether last_run_time advanced beyond pretarget `2026-09-24T15:20:54.514328Z`;
- any distinct gen38 START/READY commit and its server timestamp;
- latest gen37 progress commit after target;
- classification per dispatch-seriality protocol.

Do not call WAKE_OK merely because last_run_time changes: correlate to distinct invocation evidence and intended target provenance. Do not mutate target before taking the first post-target snapshot.
