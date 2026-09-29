---
name: oystro-approve
description: 'Human Gate 1 — approve the spec/design before implementation begins'
persona: Gatekeeper
status: active

notes: |
  Human Gate 1. After /oystro:define produces the spec (spec.yaml, with design and tasks
  internalised), the human explicitly approves it before /oystro:build may run.

gates:
  - check: 'spec.yaml exists'
    on_fail: STOP, run /oystro:define
  - check: 'spec.yaml "Ref: PENDING"'
    on_fail: STOP, already approved or rejected

actions:
  - present_spec_and_design_to_human
  - wait_for_explicit_human_approval
  - if_approved:
    - set_ref: 'APPROVED (in spec.yaml)'
    - prompt_testing_level: confirm or select the feature Testing Level (S/M/L)
    - route: to /oystro:build
  - if_changes_requested:
    - set_ref: 'REJECTED (in spec.yaml)'
    - route: to /oystro:define

must_do:
  - Present the full spec (design and tasks included) to the human
  - Wait for explicit human approval
  - Confirm or select the Testing Level (S/M/L)
  - Update the Ref marker only after human approval

must_not_do:
  - Approve without human confirmation
  - Proceed to build without Ref: APPROVED
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->
