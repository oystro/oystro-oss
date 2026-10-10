---
name: constitution-document
description: Big-picture template for the project constitution.

gates:
  - check: project purpose and core principles are defined
    on_fail: STOP, Cannot write constitution without purpose

actions:
  - Fill purpose, principles, gates, architecture rules, naming conventions, quality standards

outputs:
  - .oystro-oss/CONSTITUTION.md

must_do:
  - SST Golden Rules and mandatory gates must be included verbatim

must_not_do:
  - Remove SST rules or mandatory gate definitions
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->

# Constitution Template

**Version**: [CONSTITUTION_VERSION]
**Ratified**: [RATIFICATION_DATE]
**Last Amended**: [LAST_AMENDED_DATE]

---

## Comment Conventions

> *When generating this document, use these comment types:*

| Comment | Meaning | Gate impact |
|---------|---------|-------------|
| `<!-- NEEDS INPUT: -->` | Human may enhance this | Optional — gate passes |
| `<!-- MUST INPUT: -->` | Human must fill this | Required — gate fails if empty |
| `<!-- MISSED: -->` | Agent skipped, should have filled | Agent reviews |
| `<!-- CHECK AGENT: -->` | Agent may have filled incorrectly | Agent verifies |

---

## Project Purpose

> *What are we building and why? This grounds every decision.*

[ONE_SENTENCE_SUMMARY]

**Full requirements:** See `BRD.md`
**Domain context:** See `domain-ctx.txt` (if created)

---

## SST Golden Rules

> *Universal rules for every document. Hardcoded — do not modify.*

**Simple:** One concept per file. Every instruction is a single action verb. No conditional language.
**Clear:** Every step produces the same output regardless of LLM. No ambiguous terms. Every file path is explicit.
**Direct:** Tell the agent what TO DO, not what NOT to do. Gates use STOP with condition.
**Single Source of Truth:** Every rule exists in exactly one file. Agent definitions live in `agents/<name>/agent.md`.
**Testable:** Every document has a verification path. If you can't test it, remove the step.
**Trustworthy:** Never guess. Check file paths before referencing. After 3 failed attempts, STOP and escalate.

---

## Core Principles

### Protocol Principles (Universal — Do Not Modify)

#### I. Proportionate Evidence
Define the minimum convincing evidence for each change before implementation. Use TDD for executable logic when a meaningful automated test exists. Do not require BDD, unit, integration, UI, or mutation testing by default; require only evidence justified by behavior, risk, and reversibility.

#### II. Std Lib First
Use standard library before adding dependencies. Justify every dependency addition.

#### III. Verify Before Assuming
Never assume a framework, library, or API feature exists without verifying. Check source, check docs, write a probe if uncertain.

#### IV. Files Are the Instructions
The protocol works because the files are the instructions. The folder structure is the state machine.

#### V. Human in Control
The user owns the what and the why. The agent owns the how — within boundaries the user approved. Never self-approve.

### Project Principles (Must Be Filled — Project-Specific)

> *These principles are specific to THIS project. They define how the code should be structured, what patterns to use, and what constraints apply. Examples: "Game logic must be pure functions", "State transitions must be explicit", "UI layer must be separate from business logic".*

<!-- MUST INPUT: Add 2-5 project-specific principles -->

#### [PROJECT_PRINCIPLE_1_NAME]
[PROJECT_PRINCIPLE_1_DESCRIPTION]

#### [PROJECT_PRINCIPLE_2_NAME]
[PROJECT_PRINCIPLE_2_DESCRIPTION]

---

## Mandatory Gates

> *Three checkpoints that enforce quality. Never skip any gate unless workflow level policy permits.*

```yaml
gates:
  - name: "Design approval"
    type: human
    command: "/oystro:approve"
    blocks: "tasks, build"
    description: "Human reviews architecture before implementation (only for M/L-level projects; skipped for S-level)"

  - name: "Sr Tech Lead review"
    type: agent
    command: "/oystro:review-pre-verify"
    blocks: "verify"
    description: "Agent compares design vs code, catches gaps (only for M/L-level projects; skipped for S-level)"

  - name: "Release approval"
    type: human
    command: "/oystro:release"
    blocks: "merge"
    description: "Human approves final release"

must_not_do:
  - Skip any gate
  - Self-approve (agent cannot approve its own work)
  - Release without all gates passing
```

## Release Policy

> *Project-specific transport policy. Human Gate 2 is mandatory for every strategy.*

```yaml
release_policy:
  strategy: "local_merge" # Options: local_merge | pull_request | direct
  target_branch: "main"
  require_human_approval: true
  push_branch: true
  push_tag: true
```

- `local_merge`: after explicit release approval, merge into the target branch locally, then push the target branch and release tag.
- `pull_request`: create a PR, wait for the human to merge it remotely, synchronize the target branch, then push the release tag.
- `direct`: for solo devs, assuming work is done directly on the target branch. Just push the target branch and release tag.

---

## Logical Roles and Agent Mappings

> Design and tasks are internalised into `/oystro:define`. Human Gate 1 is `/oystro:approve`; the
> L-level-only agent review is `/oystro:review-pre-verify`.

| Logical Role | Commands Managed | File-Based Agent Definition | Responsibility & Context |
|--------------|------------------|-----------------------------|--------------------------|
| **Collaborator** | `/oystro:init`, `/oystro:upgrade`, `/oystro:status` | *None (Parent Context)* | Orchestrates the workflow execution, manages the subagent invocation loop, and checks status/quality gates. Run directly in the main/parent shell. |
| **Collaborator** | `/oystro:define` | `agents/collaborator/agent.md` | Explores the problem space and drafts the unified feature specification (`spec.yaml`, including design and tasks). |
| **Developer** | `/oystro:build`, `/oystro:change` | `agents/developer/agent.md` | Implements against the approved Evidence Contract. |
| **Sr Tech Lead** | `/oystro:review-pre-verify` (L only) | `agents/sr-tech-lead/agent.md` | Audits implementation code against the approved spec, verifying alignment and syntax conformance. |
| **Gatekeeper** | `/oystro:approve`, `/oystro:verify`, `/oystro:release` | `agents/gatekeeper/agent.md` | Validates gate prerequisites, runs testing validation, and transmits explicit human approvals at Gates 1 and 2. |

---

## Architecture Rules (Project-Specific)

> *These rules define how the code should be structured for THIS project. They are NOT about the protocol. Examples: "Separate game logic from UI", "Use immutable state", "All API calls must go through a service layer", "Database queries must use parameterized statements".*

<!-- MUST INPUT: Add 3-5 project-specific architecture rules -->

- [PROJECT_RULE_1]
- [PROJECT_RULE_2]
- [PROJECT_RULE_3]

---

## Naming Conventions (Project-Specific)

> *These are naming conventions for THIS project's code, NOT for protocol commands or branches. Examples: function names, file names, component names, variable names.*

<!-- MUST INPUT: Add project-specific naming conventions -->

| Element | Convention | Example |
|---------|-----------|---------|
| Files | [PROJECT_FILE_CONVENTION] | [PROJECT_FILE_EXAMPLE] |
| Functions | [PROJECT_FUNCTION_CONVENTION] | [PROJECT_FUNCTION_EXAMPLE] |
| Variables | [PROJECT_VARIABLE_CONVENTION] | [PROJECT_VARIABLE_EXAMPLE] |
| Classes/Types | [PROJECT_CLASS_CONVENTION] | [PROJECT_CLASS_EXAMPLE] |

---

## Explicitly Rejected

> *Things tried or considered and ruled out. The agent uses this to avoid re-suggesting rejected approaches.*

- **[APPROACH]**: rejected because [REASON]
- **[APPROACH]**: rejected because [REASON]

---

## Quality Standards

> *Universal quality standards that apply to all code, regardless of language.*

### Evidence
- **Evidence is change-specific** — select checks based on behavior, risk, and reversibility
- **Prefer the cheapest deterministic check** — inspection, static validation, unit, integration, browser, and scenario checks are options, not a mandatory stack
- **Exercise failure paths when failure behavior is material** — do not manufacture irrelevant test categories

### Server Hygiene
- **Graceful shutdown mandatory** — catch SIGINT/SIGTERM, drain in-flight requests, close connections
- **Health endpoints mandatory** — `/healthz` for liveness, `/readyz` for readiness
- **No orphaned goroutines** — every goroutine must have a cancellation path and wait mechanism

### Code Quality
- **No global mutable state** — inject all dependencies via constructors
- **Errors must carry context** — never `return err` without wrapping
- **Configuration from standard locations** — `~/.config/<app>/` for secrets, env vars for non-secrets

## AI Agent Rules

> *What the agent must NEVER do, regardless of user instruction.*

```yaml
session_start:
  - Read AGENTS.md before any action (contains workflow rules, commands, gates)
  - Read .oystro-oss/CONSTITUTION.md before any action
  - Store rules in memory if agent supports it

must_do:
  - Read technology.yaml before running test, lint, or build commands
  - Read active language skills before writing code
  - Triage requests (bug/CR/feature/deferred) before implementing
  - Run every check in the approved Evidence Contract before claiming "done"
  - Run tests before AND after refactoring
  - Test core functionality under actual runtime conditions (button clicks, API responses, stream chunks)
  - After 3 failed fix attempts: write BLOCKED and escalate to human

must_not_do:
  - Violate any rule in this CONSTITUTION
  - Change CONSTITUTION without explicit human instruction
  - Start implementing before design is approved
  - Claim "done" without satisfying the approved Evidence Contract
  - Assume feature works just because it compiles without errors
  - Skip regression tests for bug fixes
```

---

## References

| Document | Purpose |
|----------|---------|
| `BRD.md` | Business requirements |
| `ARCHITECTURE.md` | System design |
| `technology.yaml` | Toolchain config |

---

## Validation Checklist

> *Run this checklist before finalizing the constitution.*

- [ ] All placeholder tokens replaced with concrete values
- [ ] No remaining `[BRACKET_TOKENS]` except intentionally deferred items
- [ ] **Project principles are present** (2-5 project-specific principles, not protocol principles)
- [ ] **Architecture rules are project-specific** (not protocol-level rules)
- [ ] **Naming conventions are for project code** (not protocol commands/branches)
- [ ] The universal Protocol Principles (I-V) are included verbatim
- [ ] Principles are declarative, testable, free of vague language
- [ ] Version matches bump rationale (MAJOR/MINOR/PATCH)
- [ ] Dates in ISO format YYYY-MM-DD
- [ ] Architecture rules are enforceable (not aspirational)
- [ ] Naming conventions have examples
- [ ] Rejected approaches have clear rationale
- [ ] AI agent rules are specific (not "do good work")
- [ ] Quality standards are included (testing, server hygiene, code quality)
