# workwork Overlap Evidence Intake — CORRECTED

Status: PRIOR_OVERLAP_CLAIM_INVALIDATED_BY_SERVER_CLOCK_AUDIT
Correction date: 2026-09-24

## Prior claim
The earlier intake treated model-written fields in `amzsdq/workwork/overlap/OVERLAP-15M-WAKE12M-01/*.json` as clock truth and concluded that the same recurring automation had 198s of concurrent execution.

That conclusion is invalid under Mer's clock rule.

## GitHub-server audit
The relevant workwork files have authoritative GitHub commit timestamps:
- `primary-start.json` commit `5759115c21307c81cb1bef799451b35cb3869006` created `2026-09-22T16:17:32Z`.
- `primary-end.json` commit `4ca1d8f3f0881be219802627ecfa793ad6772683` created `2026-09-22T16:18:57Z`.
- `observer-01.json` commit `45107f419a3c73522af6ce1166d95e875a8047ab` created `2026-09-22T16:29:46Z`.

Therefore the durable server-clock interval from primary START file creation to primary END file creation is only 85 seconds. The observer evidence was committed 649 seconds after the primary END commit.

The JSON body fields claimed `primary_end_kst=01:32:40` and `observer_start_kst=01:29:22`, but `primary-end.json` itself was committed at 01:18:57 KST — more than thirteen minutes before its claimed end. Those model-written clock strings cannot establish runtime duration or overlap.

## Corrected interpretation
- WORKWORK SAME-CANONICAL OVERLAP: NOT PROVEN by this probe.
- WORKWORK 198s CONCURRENCY: INVALIDATED as evidence because it depends on non-authoritative model-written timestamps contradicted by GitHub server chronology.
- The probe may still contain useful qualitative ideas about overlap/handoff design, but it cannot serve as empirical proof of concurrent execution.

This does not prove serialization. It removes a false positive prior. Mer must directly observe overlap/serialization using external/server timestamps and actual distinct invocation evidence.

## Consequence for Mer
The previous high-ceiling overlap baseline loses its empirical support from this workwork probe. Do not preserve overlap architecture merely because of the old result.json classification. Current B03 same-canonical dispatch observation becomes more important: classify only from Mer durable START/END/server timestamps plus automation metadata, never model clock strings.

## Remaining design prior
Distributed leader/fencing patterns still support exactly-one authoritative owner when concurrent actors actually exist. They do not establish that ChatGPT scheduled invocations overlap.

## Falsification discipline
Future overlap evidence requires all of:
1. predecessor durable START with external/server timestamp;
2. successor distinct invocation durable START with external/server timestamp;
3. predecessor durable progress or END with external/server timestamp strictly after successor START;
4. same canonical identity proven independently;
5. no reliance on model-written start/end strings for temporal ordering.
