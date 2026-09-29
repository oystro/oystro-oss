---
name: oystro-verify
description: Verify production-ready
persona: Gatekeeper
gates:
  - check: 'review-pre-verify.md "Ref: APPROVED" (L-level only)'
    on_fail: STOP, route to /oystro:review-pre-verify
  - check: all tasks complete (if workflow_level != S; ABSENT defaults to M/L)
    on_fail: STOP, route to build
  - check: all required evidence passes
    on_fail: STOP, fix evidence failures

actions:
  - check_evidence_reuse: check if config_hash, environment_hash, and revision match the previous verification record; if unchanged, reuse recorded evidence and skip re-running tests
  - read_deferred_ledger: if deferred.md exists in active feature, read it for reporting
  - run_evidence_contract_checks: 'for S, run only the happy path or cheapest deterministic inspection; no unit-test quota'
  - run_existing_regression_suite: (run once, only when level/risk requires it for S)
  - if evidence_fails: STOP, fix evidence failures
  - check_readme_updated (if user-facing changes)
  - check_release_notes_updated
  - report_deferred_items: include a "Deferred Items" section in the verification output listing open items with IDs and destinations
  - write_verification (Release Ref: PENDING)
  - commit_verification
  - route: to /oystro:release (human gate)

must_do:
  - Reuse recorded evidence when config_hash, environment_hash, and revision are unchanged
  - All required evidence must pass
  - Existing regression suite must not regress
  - README updated if user-facing changes
  - RELEASE-NOTES.md updated
  - Include Release Ref: PENDING
  - Report deferred items separately from required evidence

must_not_do:
  - Skip required evidence
  - Write verification if tests fail
  - Skip documentation checks
  - Block verification for unresolved deferred items unless promoted to blocker
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->
