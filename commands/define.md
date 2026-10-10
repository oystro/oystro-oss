---
name: oystro-define
description: Collaborator persona - create unified feature spec.yaml from BRD

persona: Collaborator
goal: Produce a unified, machine-readable spec.yaml document that explicitly follows templates/spec.yaml.

gates:
  - check: BRD.md exists
    on_fail: STOP, run /oystro:init
  - check: CONSTITUTION.md exists
    on_fail: STOP, run /oystro:init

preflight:
  - read_brd (requirements, goals, constraints)
  - read_constitution (rules, conventions)
  - read_previous_feedback (to act on gap list or rejection notes if returning from failure)

actions:
  - resolve_active_phase: use .oystro-oss/plan.yaml
  - create_feature_folder: use active feature folder
  - identify_feature_from_brd
  - classify_change: documentation, configuration, executable_logic, workflow, bug_fix, business_behavior, ui_behavior, security_boundary, migration, or prototype
  - perform_adaptive_behavior_discovery: use examples only when behavior, state transitions, or user outcomes require clarification
  - propose_behavior_model: identify actors, goals, initial conditions, event flow, state transitions, and invariants
  - identify_primary_functional_flow: name the primary happy-path functional flow or justify inspection as stronger and cheaper
  - propose_testing_level: select S/M/L from change classification, risk, and boundary depth
  - propose_workflow_level: select S/M/L from risk, size, reversibility, and stack familiarity
  - propose_locked_constraints: identify user-specified technology constraints
  - propose_state_classification: classify persisted state by authority (model, operational, curated, external authoritative)
  - present_behavior_playback_to_human: ask bounded, contextual questions about load-bearing uncertainties
  - for_each_requirement:
    - create_testable_acceptance_examples: use Given/When/Then only for material behavior
    - identify_acceptance_criteria
  - design_architecture: for M/L workflow levels, detail the technical decisions, component mapping, and file layout
  - assess_feature_skills: assess detected stack and feature task needs to consult or install only the specific required maintained skills from oystro-oss-skills (never install the full registry)
  - write_granular_tasks: for M/L workflow levels, break design into granular tasks with dependencies
  - write_spec: write spec.yaml using templates/spec.yaml containing spec metadata, behavioral stories, design decisions, and tasks
  - set_ref: PENDING
  - route: to /oystro:build (design and tasks are now internal to define)

outputs:
  - spec.yaml with Ref: PENDING (including workflow_level, testing level, constraints, state classification, design, tasks, and decisions)

must_do:
  - Classify the change before selecting evidence
  - Perform behavior discovery when behavior is material or uncertain
  - Mark statements as confirmed, observed, inferred, or assumed
  - Present a concise behavior playback to the human before writing spec
  - Make acceptance criteria testable without forcing one notation
  - Cover all acceptance criteria
  - For M/L levels, perform design architecture and write granular tasks internal to the define command flow
  - Assess feature stack and task needs to pull or load only required maintained skills; never install the whole registry
  - Define one primary happy-path functional flow or justify inspection as stronger and cheaper
  - Propose S/M/L workflow level and testing level from risk and boundary depth
  - Get human review of spec
  - Write locked technology constraints and state classification to spec.yaml
  - Regenerate the derived handover snapshot after spec is written (Oystro runtime)

must_not_do:
  - Skip requirements
  - Assume implementation details
  - Proceed without human approval
  - Install all skills unconditionally, dump the full skill registry, or bulk install unneeded skills
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->
