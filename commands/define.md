---
name: oystro-define
description: Collaborator persona. Start a phase or a slice. Write the unified feature spec.yaml from the BRD.

persona: Collaborator
delegation: subagent-preferred  # reads the BRD; keep the main context clean
goal: Produce one machine-readable spec.yaml that follows templates/spec.yaml.

gates:
  - check: BRD.md exists
    on_fail: STOP. Run oystro:init.
  - check: CONSTITUTION.md exists
    on_fail: STOP. Run oystro:init.

preflight:
  - read_brd: Read the requirements, the goals, and the constraints.
  - read_constitution: Read the rules and the conventions.
  - read_previous_feedback: Read the gap list or the rejection notes if you return from a failure.

actions:
  - resolve_active_phase: Create or update .oystro-oss/plan.yaml from templates/plan.yaml. Set the active phase and add this slice. Create the phase integration branch `phase/<phase-id>` when it is absent, and create the slice branch `feature/<FID>` from it.
  - create_feature_folder: Use the active feature folder.
  - identify_feature_from_brd
  - classify_change: documentation, configuration, executable_logic, workflow, bug_fix, business_behavior, ui_behavior, security_boundary, migration, or prototype
  - perform_adaptive_behavior_discovery: Use examples only when the behavior, the state transitions, or the user outcomes need clarification.
  - propose_behavior_model: Identify the actors, the goals, the initial conditions, the event flow, the state transitions, and the invariants.
  - identify_primary_functional_flow: Name the primary happy-path flow, or justify inspection as stronger and cheaper.
  - read_testing_level: Take the project testing level (Light/Standard/Full) from CONSTITUTION.md.
  - read_workflow_level: Take the project workflow level (S/M/L) from CONSTITUTION.md. Level S skips the design, the tasks, and the L-only review. No level skips the human approval gate.
  - propose_locked_constraints: Identify the user-specified technology constraints.
  - propose_state_classification: Classify the persisted state by authority (model, operational, curated, external authoritative).
  - present_behavior_playback_to_human: Ask bounded questions about the load-bearing uncertainties.
  - for_each_requirement:
    - create_testable_acceptance_examples: Use Given/When/Then only for material behavior.
    - identify_acceptance_criteria
  - design_architecture: For the M and L levels, detail the technical decisions, the component map, and the file layout.
  - write_granular_tasks: For the M and L levels, break the design into granular tasks with dependencies.
  - write_spec: Write spec.yaml from templates/spec.yaml. Include the spec metadata, the behavioral stories, the design decisions, and the tasks.
  - set_ref: PENDING
  - route: Route to oystro:build. The design and the tasks are now internal to define.

outputs:
  - .oystro-oss/plan.yaml: the active phase and the slice.
  - 'spec.yaml with Ref: PENDING. Create it according to the CONSTITUTION.md project policy. Include the constraints, the state classification, the design, the tasks, and the decisions.'

must_do:
  - Classify the change before you select the evidence.
  - Cover the domain model from CONSTITUTION.md: the actors, the user journeys, the domain events, and the actions that the feature touches.
  - Discover the behavior when it is material or uncertain.
  - Mark each statement as confirmed, observed, inferred, or assumed.
  - Present a short behavior playback to the human before you write the spec.
  - Make each acceptance criterion testable. Do not force one notation.
  - Cover all acceptance criteria.
  - For the M and L levels, write the design and the granular tasks inside the define flow.
  - Define one primary happy-path flow, or justify inspection as stronger and cheaper.
  - Take the S/M/L workflow level and the Light/Standard/Full testing level from CONSTITUTION.md.
  - Get a human review of the spec.
  - Write the spec prose (the stories, the criteria, the requirements, and the decisions) in simplified technical English: one fact per sentence, the active voice, short sentences.
  - Write the locked technology constraints and the state classification to spec.yaml.
  - Regenerate the derived handover snapshot after you write the spec (Oystro runtime).

must_not_do:
  - Skip a requirement.
  - Assume an implementation detail.
  - Proceed without the human approval.
---
