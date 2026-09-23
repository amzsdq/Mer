# Priority / force interpretation rule v1

Freeze before T2 result.

## What these tests can establish
They can establish observed behavioral control under this automation/tool context:
- DIRECT_SELECTED: a directly embedded instruction controlled the harmless field despite conflicting repository content.
- DELEGATION_SELECTED: an embedded meta-instruction successfully delegated that field to fresh durable content.
- POINTER_SELECTED: a pointer-only bootstrap successfully reconstructed the field from durable content.

They cannot establish undocumented absolute platform instruction hierarchy.

## Decision matrix
1. T1=PROMPT, T2=GITHUB
   - strongest support for HYBRID:
     prompt is effective for stable meta-rules/invariants, while explicit delegation makes GitHub authoritative for dynamic domains.
2. T1=PROMPT, T2=PROMPT or non-GitHub value
   - delegation is unreliable or ambiguous; increase embedded scope or redesign durable-loading contract before adoption.
3. T1=GITHUB
   - direct embedded force hypothesis is weakened; do not assume hard invariants are safer merely because embedded.
4. T3 pointer-only succeeds under healthy GitHub but fails closed poorly under adverse bootstrap
   - pointer-only remains unsuitable for survival/scheduler invariants even if normal behavior works.
5. T3 pointer-only matches hybrid under adverse tests with lower overhead
   - reconsider hybrid; pointer-only may be sufficient.

## Promotion rule
Do not promote based on marker tests alone. Marker tests establish control routing, not continuation safety.
Final boundary needs both:
- control-routing evidence (T1/T2/T3), and
- adverse recovery/bootstrap evidence.

## Anti-overfitting
Do not change the interpretation criteria after seeing T2/T3 unless the test itself is invalid; if invalid, record INVALID and rerun with a new sample ID.
