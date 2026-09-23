# Prompt rollout protocol v1

## Problem discovered
During T1 -> T2 transition, the live automation prompt advanced to T2 while control/active.json still referenced T1. That temporarily inverted the intended authority relation (GitHub canonical should precede deployed copy).

## Required order
1. PREPARE
   - write the next canonical prompt to GitHub;
   - record desired prompt version/content identifier;
   - set rollout_state=PREPARED;
   - do not yet claim it is deployed.
2. DEPLOY
   - update the SAME automation prompt from that exact canonical candidate;
   - preserve scheduler invariants.
3. VERIFY
   - read live automation state;
   - compare the live prompt/test/version with the prepared canonical;
   - if mismatch, classify PROMPT_DEPLOY_MISMATCH and retain previous_good metadata.
4. ACTIVATE
   - only after live verification, update active.json and manifest to the deployed prompt/version;
   - set rollout_state=ACTIVE and deployed_version=desired_version.
5. ROLLBACK
   - on failed deployment/verification, restore previous known-good prompt when safe and mark rollout failure durably.

## Invariant
GitHub canonical may be ahead of deployed state (PREPARED), but deployed state must never be treated as canonical when GitHub has not first registered that candidate.

Dynamic Goal/Plan/state remain GitHub-only and are not copied into the deployed prompt.
