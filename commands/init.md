---
name: oystro-init
description: >
  Initialize oystro-oss in a project (run once). Detect the scenario, derive the documents, and
  create the scaffold. For a new phase or slice, use oystro:define.

persona: Collaborator
delegation: inline-ok  # bootstrap; run in the Collaborator context
reason: Bootstrap operation
goal: Produce clear, complete, and readable foundation documents (BRD, Architecture) from the templates in .oystro-oss/templates/.

preflight:
  - read_canonical_agents: Read the canonical AGENTS.md before you scan, clarify, create a file, or run a command.

gates:
  - check: "! .oystro-oss/ exists"
    on_fail: STOP. .oystro-oss already exists. Use oystro:upgrade.

actions:
  - scan: Find the existing documents (README, PRD, ADR, code).
  - classify_scenario: "A: greenfield. B: brownfield. C: documented."
  - assess_project_type: Decide if the project is a system, an application, a program, a feature, a bug fix, or a change request in a brownfield repository.
  - assess_domain: Identify the project domain. Probe the user for the actors, the user journeys, the domain events, the actions, the state, and the invariants. Ask until the PRD is complete.
  - derive_domain_model: Write the project context and the domain model to CONSTITUTION.md from templates/big-picture/CONSTITUTION.md.
  - if_gui_application: Do the design work during init. Seed the design registry and complete the UI brief.
  - bind_clarification_context: Treat each user answer as initialization input until this command stops.
  - create_scaffold:
    - Create the state directory `.oystro-oss/` in the project root.
    - Copy the protocol (commands/, agents/, templates/, VERSION) into `.oystro-oss/`.
    - Merge the protocol `AGENTS.md` into the root `AGENTS.md` (see `install_agents_md`). Never replace the project's own rules.
    - Create the initial state files from the matching templates:
      - Create .oystro-oss/CONSTITUTION.md.
      - Create .oystro-oss/BRD.md.
      - Create .oystro-oss/ARCHITECTURE.md.
      - Create .oystro-oss/technology.yaml. Start from templates/technology.example.yaml.
      - Create .oystro-oss/design-registry.yaml.
      - Create .oystro-oss/SLICE_LOG.md.
      - Create `VERSION` in the project root. Start at v0.1.0.
      - Create .oystro-oss/phases/active/ and .oystro-oss/phases/archive/. The plan itself is created at define.
      - Create .oystro-oss/specs/active/ and .oystro-oss/specs/done/.
  - record_generation: The Oystro engine writes the product, the schema, and the release to `.oystro-oss/manifest.json`. A protocol-only project skips this step.
  - install_agents_md: 'Merge the protocol `AGENTS.md` into the project''s `AGENTS.md`. When the project has no `AGENTS.md`, write the protocol one at the root. When it has one, put the protocol rules at the top, then the marker line `<!-- ---------- Do not modify the above part: it is managed by oystro ---------- -->`, then the project''s own agent rules. Remove from the project rules any rule that duplicates or contradicts the protocol (its workflow, gates, or command names). Keep its project-specific rules (structure, commands, off-limits paths, conventions).'
  - if_brownfield: Capture the existing `AGENTS.md`, `claude.md`, `.cursorrules`, and other agent rules into `.oystro-oss/CONSTITUTION.md` as project rules.
  - symlink_agent_configs: Symlink `claude.md`, `.cursorrules`, and `.clinerules` to the root `AGENTS.md`.
  - derive_constitution: Derive it from the project analysis and the user input. Set release_policy.strategy to local_merge by default. Let the user choose pull_request or direct.
  - prompt_levels: Ask the user for the project workflow level (S/M/L) and the testing level (Light/Standard/Full). Write both to CONSTITUTION.md.
  - derive_brd: Derive it from the PRD, the documents, or the conversation. Use templates/big-picture/BRD.md for the structure.
  - derive_architecture: Derive it from the code structure, the documents, or the conversation. Use templates/big-picture/ARCHITECTURE.md and add mermaid diagrams.
  - derive_design_registry: Create an empty .oystro-oss/design-registry.yaml for the project additions.
  - derive_technology: Derive it from the detected stack. Use templates/technology.example.yaml as the shape.
  - initialize_slice_log: Create .oystro-oss/SLICE_LOG.md.
  - derive_runtime_smoke: Add the cheapest real-entry-point smoke check to the project integration suite for an executable application.
  - present: 'Present all project-level artifacts to the human for review: CONSTITUTION.md, BRD.md, ARCHITECTURE.md, technology.yaml, design-registry.yaml, and the merged AGENTS.md. Ask the human to review and edit them, especially CONSTITUTION.md.'
  - wait: Wait for the human approval.
  - commit: Commit the initial oystro-oss setup.
  - report_next_command: Derive the earliest permitted command from the project state.
  - stop_after_init: Return control to the user. Do not invoke another workflow command.

must_do:
  - Get the human approval on all documents.
  - Copy exactly the folders and the state files in `create_scaffold`.
  - Keep the project's own agent rules below the oystro marker in `AGENTS.md`.
  - Preserve all existing agent rules (claude.md, .cursorrules, and others) in CONSTITUTION.md.
  - Symlink all alternative agent config files to AGENTS.md.
  - Explain what you derived and why.
  - Probe the user until the PRD and the domain model are complete.
  - Use the clarification answers only for the bootstrap artifacts and the configuration.
  - Stop after you report the next permitted command.

must_not_do:
  - Skip the human review.
  - Guess. Ask instead.
  - Overwrite an existing file without asking.
  - Remove a project agent rule without capturing it in CONSTITUTION.md.
  - Create an application implementation file.
  - Treat a clarification answer as authorization for another command.
  - Invoke define, design, tasks, build, verify, or release automatically.
---
