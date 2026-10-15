---
name: oystro-release
description: Release a verified phase (Human Gate 2). Archive the phase with its slices, specs, and evidence.
persona: Gatekeeper
reason: Human gate operation

gates:
  - check: the active phase has at least one slice
    on_fail: STOP. Run oystro:define to start a phase.
  - check: every slice in the active phase is verified
    on_fail: STOP. Route to oystro:verify for the unverified slice.
  - check: on the target branch (if the strategy is direct)
    on_fail: STOP. Switch to the target branch.
  - check: the constitution contains a valid release_policy strategy
    on_fail: STOP. Ask the human to choose local_merge, pull_request, or direct.

actions:
  - read_phase: read the active phase and its slices from .oystro-oss/plan.yaml.
  - collect_deferred: gather the deferred items from every slice of the phase.
  - block_on_promoted_blockers: STOP and route to the earliest affected command if a deferred item has status=promoted-to-blocker.
  - read_release_policy: read the strategy, the target_branch, require_human_approval, push_branch, and push_tag.
  - disclose: present the phase, its verified slices, their evidence, their proof docs, and the deferred items to the human.
  - wait_for_explicit_human_gate_2_approval
  - after_human_approval:
    - 'Set every slice verification.md marker from Release Ref: PENDING to Release Ref: APPROVED.'
    - Record the approval in the release commit.
  - merge_slices: merge the phase integration branch `phase/<phase-id>` into the target branch per the release_policy. Every verified slice is already merged into the phase branch.
  - archive_phase: move .oystro-oss/phases/active/<phase> to .oystro-oss/phases/archive/<phase>. The phase keeps its slices, their specs, their verification, and their deferred ledgers.
  - update: set the released phase status to "released" in .oystro-oss/plan.yaml.
  - update: update VERSION for the phase.
  - update: add a phase release entry to .oystro-oss/SLICE_LOG.md.
  - verify_release_state:
    - Confirm every archived slice verifies Release Ref: APPROVED.
    - Confirm the active phase path is gone after the archive.
    - Confirm the derived handover, VERSION, and SLICE_LOG describe the same released phase (Oystro runtime).
  - create_release_tag
  - push_release_tag_if_enabled
  - delete_slice_branches_after_merge

must_do:
  - Require every slice in the phase to be verified before the release.
  - Read release_policy from the project constitution.
  - Require explicit human approval before the phase release.
  - Archive the whole phase with its slices, specs, and evidence.
  - Set every slice marker to Release Ref: APPROVED only after the human approval.
  - Update VERSION and SLICE_LOG.md.
  - Verify the archive, VERSION, SLICE_LOG, and handover agree.

must_not_do:
  - Release a phase while a slice is unverified.
  - Merge or publish without human approval.
  - Archive a slice on its own. Archive the phase.
  - Leave an archived verification.md with "Release Ref: PENDING".
  - Block the release for an unresolved deferred item, unless it is promoted to a blocker.
---

# Release a phase

A release is a **phase** operation, not a slice operation. A phase holds many slices. Every slice is
built and verified first (with Human Gate 1 at its define). Then this command releases the phase at
Human Gate 2 and archives it.
