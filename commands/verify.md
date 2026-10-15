---
name: oystro-verify
description: Verify the work against the approved evidence and the acceptance criteria.
persona: Gatekeeper
delegation: subagent-preferred  # runs the evidence; keep the main context clean

gates:
  - check: 'review-pre-verify.md "Ref: APPROVED" (L-level only)'
    on_fail: STOP. Route to oystro:review-pre-verify.
  - check: all tasks complete (if the workflow level is not S)
    on_fail: STOP. Route to build.
  - check: all required evidence passes
    on_fail: STOP. Fix the evidence failures.

actions:
  - check_evidence_reuse: Compare config_hash, environment_hash, and revision with the previous verification record. If they are unchanged, reuse the recorded evidence and skip the test run.
  - read_deferred_ledger: Read deferred.md if it exists in the active feature.
  - run_evidence_contract_checks: For level S, run only the happy path or the cheapest deterministic check. Do not set a unit-test quota.
  - run_existing_regression_suite: Run it once, and only when the level or the risk requires it for level S.
  - refresh_derived_state: 'For a change that affects data, refresh the derived or cached state before you record the evidence. For example: a storefront cache, a search index, or a projection. Record the refresh in the proof.'
  - if evidence_fails: STOP. Fix the evidence failures.
  - check_readme_updated: Check it for a user-facing change.
  - check_release_notes_updated
  - report_deferred_items: Add a "Deferred Items" section to the verification output. List each open item with its ID and its destination.
  - write_proof: 'Write proof.md. Record the raw evidence for each scenario: the command, the exit status, the timestamp, and the raw output.'
  - write_verification: 'Write the verification with Release Ref: PENDING.'
  - commit_verification
  - route: Route to oystro:release (human gate).

must_do:
  - Reuse the recorded evidence when config_hash, environment_hash, and revision are unchanged.
  - Pass all required evidence.
  - Keep the existing regression suite green.
  - Update README if the change is user-facing.
  - Update RELEASE-NOTES.md.
  - 'Include Release Ref: PENDING.'
  - Report the deferred items separately from the required evidence.
  - Write proof.md with the raw evidence for the human gate.
  - Refresh any derived or cached state that the change affects before you record the evidence.

must_not_do:
  - Skip the required evidence.
  - Write the verification if a test fails.
  - Skip the documentation checks.
  - Block the verification for an unresolved deferred item, unless it is promoted to a blocker.
---
