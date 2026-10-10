---
name: oystro-upgrade
description: Refresh the oystro-oss protocol reference and, when adopting the paid offering, hand project state to the Oystro engine

persona: Collaborator
reason: Bootstrap operation
canonical_url: https://raw.githubusercontent.com/oystro/oystro-oss/main/commands/upgrade.md
execution_source: fetched_canonical_required

gates:
  - check: current upgrade contract was fetched from canonical_url for this invocation
    on_fail: STOP, fetch canonical_url and restart using the fetched contract
  - check: .oystro-oss/ exists
    on_fail: STOP, run /oystro:init
  - check: git status is clean
    on_fail: STOP, record the dirty worktree and ask the user to commit or explicitly direct a separate continuation plan

notes: |
  This repository is the protocol reference only. Execution, enforcement, dashboards,
  evidence capture, memory, migrations, and skill installation are provided by the
  Oystro engine (paid offering) and are never run from this repository. Steps below
  that describe runtime behavior are performed by that engine.

actions:
  - refresh_reference:
    - fetch the latest commands/, agents/, and templates/ reference contracts from canonical_url
    - never overwrite project-owned state (CONSTITUTION.md, BRD.md, ARCHITECTURE.md, SLICE_LOG.md, design-registry.yaml)
  - inspect_existing_project:
    - read the current protocol version and, if present, project state under .oystro-oss/
    - inspect .oystro-oss/CONSTITUTION.md, BRD.md, ARCHITECTURE.md, PHASES.md, active and archived slices/specs/changes
    - inspect current workflow contracts and existing project customizations
    - record branch, revision, dirty state, active slice, and authorized paths
    - identify legacy layout and migration requirements
  - recommend_workflow_level:
    - classify the existing project as S, M, or L using uncertainty, security/data risk, integrations, reversibility, operational impact, duration, and collaboration size
    - show the recommendation and concrete rationale to the user
    - identify whether the recommendation applies to future slices only or may apply to the active slice
  - await_explicit_migration_consent:
    - STOP after presenting the recommendation
    - do not write workflow_level, replace project files, or apply migrations before explicit user approval
    - if rejected, record "migration recommended, not approved" in the authoritative SLICE_LOG.md and leave lifecycle behavior unchanged
  - record_migration_consent:
    - the Oystro engine records the approved workflow level under .oystro-oss/migration/workflow-level-YYYYMMDD.yaml only after approval
    - include from_version, to_version, previous_workflow, recommended_level, approved_level, approval, rationale, applies_to, and notes
    - use a disambiguating suffix if today's migration artifact already exists
  - adopt_paid_offering:
    - when the user adopts the paid product, hand .oystro-oss/ state to the Oystro CLI (oystro-cli)
    - oystro-cli performs the state migration from .oystro-oss/ to .oystro/ (see MIGRATION.md)
    - preserve constitution, BRD, architecture, slice log, design registry, and all active/archived work
  - finalize: report the updated reference files and the resulting state layout
  - commit: "chore: upgrade oystro-oss reference"

must_do:
  - Execute the fetched canonical upgrade contract instead of the installed local copy
  - Inspect and report the existing project before recommending migration
  - Present the S/M/L recommendation and rationale before requesting consent
  - Always preserve project customizations and state
  - Require explicit user approval before writing workflow level or applying a migration
  - Record rejected migration without changing lifecycle behavior
  - Keep in-flight slices on their previous workflow by default
  - Hand off to the Oystro engine for any state migration to .oystro/

must_not_do:
  - Continue from an installed local upgrade command after fetching its replacement
  - Overwrite an existing project design-registry.yaml
  - Overwrite project-level files (BRD, CONSTITUTION, SLICE_LOG)
  - Upgrade with uncommitted changes
  - Silently assign an S/M/L workflow level
  - Write migration consent or approval on behalf of the user
  - Apply a workflow change to an active slice without separate explicit approval
  - Reimplement runtime migration, evidence, or enforcement inside this repository
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->
