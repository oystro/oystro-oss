---
name: oystro-review-pre-verify
description: Sr Tech Lead fresh-eyes review in an isolated context. Not a human gate. Applies to L-level projects only. S and M skip it. Do not run it directly.
persona: Sr Tech Lead
delegation: subagent-preferred  # isolation is the point: fresh eyes, no build memory
reason: Fresh-eyes review, isolated from the build process.

gates:
  - check: spec.yaml exists
    on_fail: STOP. Route to define (or oystro:change).
  - check: all tasks complete
    on_fail: STOP. Route to build.

preflight:
  - read_deferred_ledger: Read deferred.md if it exists in the active feature.

actions:
  - check_framework_capabilities: Reject an unjustified duplicate of a capability that the selected stack supplies.
  - check_previous_review: If review-pre-verify.md exists with a FAIL verdict, read its gap list. Verify only that the gaps are fixed, and check for adjacent regressions. Do not re-audit the whole codebase.
  - if no_previous_review_or_passed:
    - read_changed_files_and_relevant_dependencies
    - read_design: Read the architecture and the interfaces.
    - read_spec: Read the requirements and the acceptance criteria.
    - check_evidence_contract_compliance
    - reconcile_preverify_items: Check the open pre-verify deferred items against the implementation. Mark each resolved item with the resolution evidence.
    - classify_findings: Apply the blocker predicate to each new finding (concrete evidence plus invalidated approved contract plus earliest corrective command).
    - check_design_implementation_gaps
    - check_spec_requirement_gaps
    - check_required_evidence_coverage
    - check_code_quality: Check the patterns, the conventions, and the errors.
  - write_review_report: Record the verdict and the gaps by severity (critical, high, medium, low).
  - if verdict_is_PASS:
    - set_ref: APPROVED
    - route: Route to oystro:verify.
  - if verdict_is_CONDITIONAL:
    - set_ref: APPROVED (with the gap list)
    - route: Route to oystro:verify.
  - if verdict_is_FAIL:
    - set_ref: PENDING
    - STOP. Route to build.

must_do:
  - Check for an existing review-pre-verify.md. If a FAIL exists, verify only the failed gaps. Do not re-audit the whole codebase.
  - Audit the code for Ponytail YAGNI compliance. Flag an unnecessary third-party dependency or an excessive abstraction.
  - Read every source file related to the current phase or the CR or bug. Exclude the auto-generated files (for example .templ and .pb.go).
  - Verify that the project integration suite covers the changes that the evidence contract requires.
  - Verify that the project integration suite has a regression test for each bug fix.
  - Compare the code against the design and the spec.
  - Identify the gaps. Do not list suggestions.
  - Set a production-ready bar.
  - Classify each missing item as a defect, a design_gap, or an improvement.
  - Block a defect and a design gap.
  - Record an improvement as a non-blocking backlog item.
  - Reconcile the pre-verify deferred items. Mark a resolved item, and leave an unresolved item open.
  - 'Apply the blocker predicate to each new finding: concrete evidence plus invalidated approved contract plus earliest corrective command.'
  - Append each deferred finding to deferred.md with an ID, a source, a rationale, a destination, and a status.
  - Use the write tool to save the final report to disk. Do not output the report in the final chat response.

must_not_do:
  - Assume the build agent worked correctly.
  - Read a large auto-generated file (for example .templ, .pb.go, or a bundle).
  - Skip the sanity-check completeness check.
  - Give a PASS if the sanity check is incomplete.
  - Give a PASS if a gap exists.
  - Add a test category only because it is possible.
  - Expand the approved evidence contract unless a security exposure, a changed scope, or a false design assumption is proven.
  - Block a finding that lacks the full blocker predicate.
  - Route a deferred finding back to build.
---
