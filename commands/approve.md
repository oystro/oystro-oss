---
name: oystro-approve
description: Human Gate 1. Approve the spec and the design before the implementation starts.

persona: Gatekeeper
delegation: inline-ok  # human gate; keep the human exchange in the main thread
status: active

notes: |
  Human Gate 1. After oystro:define writes the spec (spec.yaml, with the design and the tasks
  internal), the human approves it. oystro:build must not run before the approval.

gates:
  - check: spec.yaml exists
    on_fail: STOP. Run oystro:define.
  - check: 'spec.yaml has "Ref: PENDING"'
    on_fail: STOP. The spec is already approved or rejected.

actions:
  - present_spec_and_design_to_human
  - wait_for_explicit_human_approval
  - if_approved:
    - set_ref: APPROVED (in spec.yaml)
    - route: Route to oystro:build.
  - if_changes_requested:
    - set_ref: REJECTED (in spec.yaml)
    - route: Route to oystro:define.

must_do:
  - Present the full spec (with the design and the tasks) to the human.
  - Wait for the explicit human approval.
  - Update the Ref marker only after the human approval.

must_not_do:
  - Approve without the human confirmation.
  - 'Proceed to build without Ref: APPROVED.'
---
