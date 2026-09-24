# Prompt rollout protocol v2

## Problem discovered
During T1 -> T2 transition, the live automation prompt advanced while durable control still referenced the prior version. A later O8 repair transition exposed a related risk: matching only PROMPT_VERSION/PROMPT_ID is insufficient if deployed text and canonical text can differ under the same identifiers.

## Required order
1. PREPARE
   - write the exact next canonical prompt text to GitHub first;
   - record desired prompt version/id and canonical blob SHA;
   - set rollout_state=PREPARED and sync_required=true;
   - do not claim deployment yet.
2. DEPLOY
   - update the SAME automation prompt from that exact canonical candidate;
   - preserve scheduler invariants and do not create a replacement automation.
3. VERIFY
   - read live automation state;
   - require same automation ID, enabled state and timing mode as intended;
   - compare live PROMPT_VERSION/PROMPT_ID **and the deployed prompt content** against the prepared canonical. Version/id equality alone is not sufficient proof of content equality;
   - if mismatch, classify PROMPT_DEPLOY_MISMATCH and retain previous_good metadata.
4. ACTIVATE
   - only after live verification, update manifest/deployment metadata to the verified canonical blob SHA;
   - set rollout_state=ACTIVE, deployed_version/id, deployed_enabled, and sync_required=false.
5. ROLLBACK / REPAIR
   - on failed deployment/verification, do not continue substantial work under an ambiguous prompt;
   - restore the previous known-good prompt when safe or repair the same canonical deployment;
   - preserve scheduler continuity and record the exact mismatch durably.

## Invariants
- GitHub canonical may be ahead of deployed state (PREPARED), but deployed state must never be treated as canonical when GitHub has not first registered that candidate.
- A manifest version/id match is a bootstrap optimization, not a license to ignore known content drift. When drift is suspected or a rollout is active, verify content identity.
- Dynamic Goal/Plan/state remain GitHub-only and are not copied into the deployed prompt.
- Scheduler WRITE_OK, prompt update acknowledgement, and verified live prompt/schedule state are separate evidence classes.
