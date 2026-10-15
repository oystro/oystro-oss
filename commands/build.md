---
name: oystro-build
description: Implement the feature against the approved Evidence Contract.
persona: Developer
delegation: subagent-preferred  # reads much and writes code; keep the main context clean

gates:
  - check: 'spec.yaml "Ref: APPROVED"'
    on_fail: STOP. Route to oystro:approve (Human Gate 1).
  - check: no BLOCKED.md in the active features
    on_fail: STOP. Route to blocked-state recovery (Oystro runtime).

preflight:
  - read_constitution_and_spec: Read the workflow level from CONSTITUTION.md. Read the requirements, the design, and the tasks from spec.yaml if the workflow level is not S.
  - read_review_pre_verify: Read the review to fix the code defects if you return from a failure.
  - read_deferred_ledger: Read deferred.md if it exists in the active feature.

actions:
  - for_each_task (if the workflow level is not S; else implement the spec requirements directly):
    - execute_ponytail_decision_ladder: Evaluate before you add a dependency or an abstraction.
    - produce_required_evidence: Follow the approved evidence contract.
    - use_tdd_for_executable_logic: Confirm the failure before the implementation when a meaningful executable test exists, for a deterministic invariant that the functional flow cannot prove.
    - commit: Commit the task.
  - resolve_deferred_items: Read each current-build deferred item. Resolve it, or reroute it with an explicit destination (pre-verify or next-cr).
  - update_ledger_status: Mark each resolved item with the resolution evidence in deferred.md.
  - after_all_tasks:
    - run_required_evidence
    - run_existing_regression_suite
    - if evidence_fails: STOP. Fix the evidence failures.
    - if evidence_passes: Route to oystro:review-pre-verify for level L, or to oystro:verify for any other level.

must_do:
  - Apply the Ponytail YAGNI framework when you write code.
  - Use the cheapest deterministic evidence that proves the task.
  - Use regression-first testing for a bug fix.
  - Use TDD only for a deterministic invariant that the functional flow cannot prove.
  - Record why a required evidence item does not apply.
  - Commit after each task.
  - Resolve each assigned current-build deferred item, or reroute it explicitly.
  - Update the ledger status with the resolution evidence.
  - Regenerate the derived handover snapshot after the build completes or fails (Oystro runtime).

must_not_do:
  - Implement executable logic without its required evidence.
  - Add a test category that the approved evidence contract excludes, unless a design gap is found.
  - Commit without the required evidence.
  - Drop a deferred item in silence.
---
