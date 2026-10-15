---
name: oystro-change
description: Unified change workflow for a bug or a CR.
persona: Developer
delegation: subagent-preferred  # keeps the main context clean

gates:
  - check: CONSTITUTION.md and spec.yaml exist (to read the project levels and the constraints)
    on_fail: STOP. Run oystro:define first.

actions:
  - classify_change: Identify if the change is a bug or a CR.
  - assess_impact: Determine the blast radius. List the affected components, the public interfaces (API, CLI, config, file format), the persistent state, the security boundaries, the users or downstream systems, and the reversibility. Record each item in the change record.
  - evaluate_promotion_criteria: Use the impact assessment. Promote to the full oystro:define flow if the change touches a locked stack, a public interface, a persistent schema, a security boundary, or an architecture boundary. Otherwise keep the change workflow.
  - record_baseline: Capture the baseline revision hash, the dirty state, and the observed behavior.
  - record_worktree_ownership: Populate worktree.yaml from templates/worktree.yaml. Mark a file to touch as authorized, an untracked file as generated, and any other file as preexisting.
  - read_prior_decisions: Read the decisions from the active spec.yaml and the past slices.
  - write_change_record: Create CHG-NNN.yaml (or CHG-NNN.md) from templates/feature/change.md. Carry forward the baseline, the delta, the decisions, and the relevant prior decisions.
  - request_human_approval: 'Present the change record. Stop for the explicit human approval (Ref: APPROVED) before you implement.'
  - implement_smallest_delta: Implement the change with the smallest code delta. If the change affects data, refresh the derived or cached state (a cache, a search index, a projection) and record the refresh.
  - run_focused_evidence: Verify the change on the affected flow and boundary only.
  - commit: Commit the change record, the worktree record, and the implementation code.
  - route: Route to oystro:verify.

must_do:
  - Promote the change to the full define flow if the promotion criteria are met.
  - Assess the impact before you select the delta and the evidence.
  - 'Get the explicit human approval of the change record (Ref: APPROVED) before you implement.'
  - Capture the dirty worktree state in the CHG-NNN baseline section before you edit.
  - Create and populate worktree.yaml before you change any code.
  - Carry forward the relevant decisions from the prior slices.
  - Use the smallest code delta to resolve the bug or the CR.
  - Write the change record in simplified technical English: one fact per sentence, the active voice.
  - Run the focused evidence on the affected boundary.
  - Regenerate the derived handover snapshot (Oystro runtime).

must_not_do:
  - Implement a change that touches a locked stack or a schema without promoting to the full define flow.
  - 'Implement the change before the change record is human-approved (Ref: APPROVED).'
  - Commit the implementation code without the focused evidence.
---
