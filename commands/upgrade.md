---
name: oystro-upgrade
description: Refresh the oystro-oss protocol. When the user adopts the Oystro engine, hand the project state over.

persona: Collaborator
delegation: inline-ok  # bootstrap; run in the Collaborator context
reason: Bootstrap operation
canonical_url: https://raw.githubusercontent.com/oystro/oystro-oss/main/commands/upgrade.md
execution_source: fetched_canonical_required

gates:
  - check: the current upgrade contract was fetched from canonical_url for this invocation
    on_fail: STOP. Fetch canonical_url and restart with the fetched contract.
  - check: the local protocol version (.oystro-oss/VERSION) differs from the latest release tag (v*) in the canonical repository
    on_fail: STOP. The protocol is already at the latest version. No upgrade is needed.
  - check: .oystro-oss/ exists
    on_fail: STOP. Run oystro:init.
  - check: git status is clean
    on_fail: STOP. Record the dirty worktree. Ask the user to commit, or to direct a separate continuation plan.

notes: |
  This repository is the protocol only. Execution, enforcement, dashboards, evidence capture,
  memory, and migrations are provided by the Oystro engine. They never run from this repository.
  The steps below that describe runtime behavior are performed by that engine.

  Version check: read the local version from .oystro-oss/VERSION. Read the latest release tag from
  the canonical repository (git ls-remote --tags https://github.com/oystro/oystro-oss.git 'v*', or
  the GitHub releases API). If the two match, stop: the protocol is current.

actions:
  - refresh_protocol:
    - Read the local version from .oystro-oss/VERSION and the latest release tag (v*) from the canonical repository.
    - Fetch the latest commands/, agents/, and templates/ contracts from canonical_url.
    - Never overwrite the project-owned state (CONSTITUTION.md, BRD.md, ARCHITECTURE.md, SLICE_LOG.md, design-registry.yaml).
  - inspect_existing_project:
    - Read the current protocol version. Read the project state under .oystro-oss/ if it exists.
    - Inspect .oystro-oss/CONSTITUTION.md, BRD.md, ARCHITECTURE.md, plan.yaml, and the active and archived slices, specs, and changes.
    - Inspect the current workflow contracts and the existing project customizations.
    - Record the branch, the revision, the dirty state, the active slice, and the authorized paths.
    - Identify the legacy layout and the migration requirements.
  - recommend_workflow_level:
    - Classify the existing project as S, M, or L. Use the uncertainty, the security and data risk, the integrations, the reversibility, the operational impact, the duration, and the collaboration size.
    - Show the recommendation and the concrete rationale to the user.
    - Identify if the recommendation applies to the future slices only, or if it can apply to the active slice.
  - await_explicit_migration_consent:
    - STOP after you present the recommendation.
    - Do not write the workflow level to CONSTITUTION.md, replace a project file, or apply a migration before the explicit user approval.
    - If the user rejects it, record "migration recommended, not approved" in SLICE_LOG.md. Leave the lifecycle behavior unchanged.
  - record_migration_consent:
    - After the approval, the Oystro engine records the workflow level in CONSTITUTION.md (project policy) and under .oystro-oss/migration/workflow-level-YYYYMMDD.yaml.
    - Include from_version, to_version, previous_workflow, recommended_level, approved_level, approval, rationale, applies_to, and notes.
    - Use a disambiguating suffix if today's migration artifact exists.
  - adopt_runtime:
    - When the user adopts the Oystro engine, hand the .oystro-oss/ state to the Oystro CLI.
    - The Oystro CLI performs the state migration.
    - Preserve the constitution, the BRD, the architecture, the slice log, the design registry, and all active and archived work.
  - finalize: Report the updated protocol files and the resulting state layout.
  - commit: "chore: upgrade oystro-oss"

must_do:
  - Skip the upgrade when the local version already matches the latest release tag.
  - Execute the fetched canonical upgrade contract, not the installed local copy.
  - Inspect and report the existing project before you recommend a migration.
  - Present the S/M/L recommendation and the rationale before you request the consent.
  - Always preserve the project customizations and the state.
  - Require the explicit user approval before you write the workflow level or apply a migration.
  - Record a rejected migration without a change to the lifecycle behavior.
  - Keep the in-flight slices on their previous workflow by default.
  - Hand off to the Oystro engine for any state migration.

must_not_do:
  - Refresh the protocol when the local version already matches the latest release tag.
  - Continue from an installed local upgrade command after you fetch its replacement.
  - Overwrite an existing project design-registry.yaml.
  - Overwrite the project-level files (BRD, CONSTITUTION, SLICE_LOG).
  - Upgrade with uncommitted changes.
  - Assign an S/M/L workflow level in silence.
  - Write the migration consent or the approval on behalf of the user.
  - Apply a workflow change to an active slice without a separate explicit approval.
  - Reimplement the runtime migration, the evidence, or the enforcement inside this repository.
---
