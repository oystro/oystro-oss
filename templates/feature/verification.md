---
name: verification
description: >
  Verification report for completed implementation.
  Created after build and review.
  Must include **Release Ref**: PENDING for gate tracking.
agent_contract:
  prerequisites:
    - id: PRE-001
      action: "Confirm review-pre-verify.md has Ref: APPROVED."
      on_failure: "STOP: Cannot verify without approved review."
  actions:
    - id: ACT-001
      action: "Fill acceptance criteria status table with story-by-story test results."
    - id: ACT-002
      action: "Run every check required by the approved Evidence Contract."
  must_do:
    - id: MUST-001
      action: "Include Release Ref: PENDING at top."
  must_not_do:
    - id: NEVER-001
      action: "Do not write verification without running actual tests."
  outputs:
    - id: OUT-001
      path: "verification.md with Release Ref: PENDING"
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->

# Verification Template: [FEATURE_NAME]

**Feature**: [FEATURE_NAME]
**Date**: [DATE]
**Branch**: [BRANCH]
**Release Ref**: [APPROVED|PENDING]

**Input**: the active `spec.yaml` (acceptance criteria and task list)

---

## Acceptance Criteria Results

| Story | Criterion | Status | Evidence |
|-------|-----------|--------|----------|
| US1 | Given [state], When [action], Then [outcome] | ✅ PASS / ❌ FAIL | [test name] |
| US2 | Given [state], When [action], Then [outcome] | ✅ PASS / ❌ FAIL | [test name] |

---

## Evidence Contract Results

| Required Evidence | Result | Command or Artifact |
|-------------------|--------|---------------------|
| [approved evidence] | PASS / FAIL | [reproducible evidence] |

**Non-applicable checks:** [checks intentionally excluded by the approved Evidence Contract and why]

## Regression Suite Results

- **Full suite**: ✅ All passing / ❌ N failing
- **Race detector**: ✅ CLEAN / ❌ RACE DETECTED / N/A
- **Coverage**: N% (only if the approved Evidence Contract sets a coverage threshold)
- **Linter**: ✅ CLEAN / ❌ N issues

---

## Deviations from Design

> *List any implementation decisions that differ from the approved design.*

| Deviation | Why | Impact |
|-----------|-----|--------|
| [DEVIATION] | [REASON] | [IMPACT] |

---

## Quality Checklist

**Acceptance:**
- [ ] All approved acceptance criteria pass
- [ ] All functional requirements met
- [ ] Success criteria achievable

**Code Quality:**
- [ ] All Evidence Contract checks pass
- [ ] Existing regression suite passes
- [ ] Race detector clean (if concurrent code)
- [ ] Coverage meets threshold (if required by Evidence Contract)
- [ ] Linter clean
- [ ] Constitution rules followed

**Completeness:**
- [ ] All tasks in the spec's task list marked done
- [ ] No failing tests
- [ ] No BLOCKED items remaining
- [ ] Documentation updated (if applicable)

---

## Sign-Off

- [ ] All acceptance criteria verified
- [ ] Code reviewed
- [ ] Ready for release
