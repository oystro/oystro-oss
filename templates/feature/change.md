---
name: change
description: >
  Unified Change Record template for bugs and CRs.
  Created during oystro:change workflow.
agent_contract:
  prerequisites:
    - id: PRE-001
      action: "Identify whether change is bug or CR."
  outputs:
    - id: OUT-001
      path: "CHG-NNN.md change record"
---

# Change Record: CHG-[NNN]

> Write this record in simplified technical English. Write one fact per sentence. Use the active
> voice and the present tense.

**ID**: CHG-[NNN]
**Type**: [bug|CR]
**Status**: [active|verification-pending|resolved|archived|blocked]
**Created**: [YYYY-MM-DD]
**Ref**: [APPROVED|PENDING]

---

## Baseline
- **Revision**: [baseline_git_commit_hash]
- **Dirty worktree**:
  - [path]
- **Observed behavior**: [What is currently happening]
- **Reproduction / Current behavior**: [Steps to reproduce or current state]
- **Affected flow / boundary**: [e.g., database-auth]

---

## Request
- **Expected behavior**: [What should happen]
- **Desired delta**: [For CRs: the requested enhancement]

---

## Impact
- **Affected components**:
  - [path or component]
- **Affected public interfaces**:
  - [API, CLI, config, or file format]
- **Affected persistent state**:
  - [schema or data]
- **Affected security boundaries**:
  - [boundary]
- **Affected users or downstream systems**:
  - [user or system]
- **Reversibility**: [easy | hard | one-way]
- **Decision**: [keep the change workflow, or promote to the full define flow]

---

## Analysis & Decisions
- **Root cause**: [For bugs: analysis of root cause]
- **Risk level**: [Low|Medium|High]
- **Technical Decisions**:
  - id: DEC-[NNN]
    decision: [decision_text]
    rationale: [rationale_text]
- **Assumptions**:
  - [assumption_text]
- **Relevant prior decisions**:
  - id: DEC-[XYZ]
    ref: [path_or_link_to_past_slice_spec_or_design]

---

## Acceptance Criteria
- **Functional**:
  - [ ] [verifiable_acceptance_criterium_1]
- **Invariants**:
  - [ ] [invariant_criterium_1]

---

## Implementation
- **Changed files**:
  - [path]
- **Commits**:
  - [commit_hash]

---

## Evidence
- **Verification commands**:
  - [command]
- **Result**: [pending|pass|fail]
- **Recorded at**: [ISO_timestamp]
- **Log path**: [path_to_log]
