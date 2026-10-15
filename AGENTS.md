# oystro-oss — Agent Instructions

Oystro OSS is a file-based Agentic Development Lifecycle (ADLC) protocol for AI coding agents.
It defines the workflow, the templates, the gates, and the personas.

This file is the entry point. It gives the principles and the workflow rules.
The command files give the method for each command. Read them and follow them.

## Golden Principles

Three principles override convenience.

1. **Finish the intent. Do not dilute the scope.** A fast, partial result is a failure. Complete
   the task, or stop and state what is unresolved.
2. **Give grounded answers. Do not agree to please.** State what the evidence, the code, and the
   tests show. Disagree when the user is wrong.
3. **Do the critical path first.** Find the highest-risk, least-proven dependency. Work on it
   first. Do not let other work delay it.

## Runtime Boundary

This repository ships no executor. No script, hook, or server enforces the workflow here.
The AI agent follows these conventions. The human is the only enforcement gate.

A rule that uses "must" states a protocol requirement. The agent must apply it.

## Session Start (Mandatory)

Do these steps before you act in this project.

1. Read `AGENTS.md` (this file).
2. Read `.oystro-oss/CONSTITUTION.md`.
3. Store both rule sets in memory if your agent supports memory.

## Conversational Style

- Keep answers short. Use technical prose. Do not use filler text.
- Do not use emojis in commits, issues, PR comments, or code.
- Answer the question first. Make the edits second.
- State whether you agree or disagree with user feedback. Explain the change after that.

## Workflow

The lifecycle is a fixed sequence. Two human gates stop the sequence.

```
init → define → [Gate 1: approve] → build → [review-pre-verify: L only] → verify → [Gate 2: release]
```

Run `init` on first use. You can also say "initialise this project using oystro-oss".

Every command has a command file. Read the command file before you run the command:
`.oystro-oss/commands/<command>.md` (in this repository, `commands/<command>.md`).
The table below is a summary only. Do not copy the command steps into another file.

| Command | Purpose | Gate |
|---|---|---|
| `init` | Scan the docs (run once). Derive the constitution, the BRD, the architecture, and the scaffold. | — |
| `define` | Write `spec.yaml` from the BRD. Include the design and the tasks. | — |
| `approve` | Present the spec to the human. Record the decision. | Human Gate 1 |
| `build` | Implement each task against the approved Evidence Contract. Make one commit per task. | — |
| `review-pre-verify` | Review the build against the spec. | Agent review (L only) |
| `verify` | Run the approved evidence. Check the acceptance criteria. Write the verification. | — |
| `release` | Release a verified phase. Archive it. | Human Gate 2 |
| `change` | Run the bug and CR workflow. | Human |
| `status` | Show the project state. Read only. | — |
| `upgrade` | Refresh the protocol. Hand the state to the Oystro engine. | — |

Design and tasks are internal to `define`. There are no separate design, tasks, or pre-build-review
commands.

### Workflow Levels (S / M / L)

The project sets one workflow level in `CONSTITUTION.md`. Every `spec.yaml` follows it.
A level changes the ceremony only. It does not change the human gates.

| Level | Design and tasks | `review-pre-verify` | Evidence |
|---|---|---|---|
| S | skip | skip | the happy path, or the cheapest deterministic check |
| M | required | skip | the full evidence contract |
| L | required | required | the full evidence contract |

### Phases and Slices

A project has phases. A phase has slices. A phase is the release unit. A slice is the work unit.
`define` starts a slice and adds it to the active phase.

Each phase has an **integration branch**: `phase/<phase-id>`. A slice is built on a branch
`feature/<FID>` created from the phase branch. A verified slice merges into the phase branch. The
phase release merges the phase branch into the target branch. Create the phase branch with
`define` when it is absent.

### Gates

Two human gates apply.

- **Gate 1 — `approve`.** After `define` writes a slice spec, the human approves `Ref: APPROVED`
  in `spec.yaml`. Do not start `build` for that slice before this approval.
- **Gate 2 — `release`.** After every slice of a phase is verified, the human approves the phase
  release. The slices carry a proof doc (the raw evidence from `verify`). `release` then archives
  the whole phase: its slices, their specs, their proof docs, and their evidence. Do not merge or
  archive before this approval.

`review-pre-verify` is an agent review. It is not a human gate. It applies to L-level projects only.

## Workflow Rules (Non-Negotiable)

1. Read `.oystro-oss/CONSTITUTION.md` before every action.
2. Do not start `build` before the human approves the spec (Gate 1). This rule applies to every
   level, including S.
3. Write each test in the same commit as the code that it proves. Use TDD only for a deterministic
   invariant that the functional flow cannot prove.
4. Do not fill `verification.md` before the approved Evidence Contract and the regression suite
   pass.
5. Run the full integration suite before `verification.md` for the M and L levels. Run only the
   focused checks during `build`.
6. Do not move a feature to `done/` without a passing `verification.md`.
7. After three failed fix attempts, write a BLOCKED section, stop, and ask the human.
8. Create or switch to the correct git branch before you change a file. Build a slice on
   `feature/<FID>`, branched from the phase integration branch `phase/<phase-id>`. Merge a verified
   slice into the phase branch.
9. Make one git commit per task in the approved task list.
10. Do not commit a failing test.
11. Append to `SLICE_LOG.md` on each meaningful commit.
12. Triage each request first: bug, CR, feature, or deferred.
13. Do not release a phase without explicit human approval. After the approval, set every slice's
    `Release Ref: APPROVED`, archive the phase with its slices, specs, and proof, update `VERSION`
    and `SLICE_LOG.md`, and regenerate `handover.yaml`.
14. Keep the locked intent. Use the technology, library, model, strategy, data source, cost model,
    execution mode, or persistence mechanism that the user names. Do not substitute it. If you
    cannot use it, stop and ask.
15. Keep the delegation context small. A subagent receives the task packet only: the objective, the
    allowed paths, the constraints, the evidence, the prohibited changes, and the stop conditions.
    Send no project history by default.

> The top part of this file is managed by oystro. Do not modify above the marker line
> `<!-- ---------- Do not modify the above part: it is managed by oystro ---------- -->`.
> Put project-local rules below it, and in `.oystro-oss/CONSTITUTION.md`.

## Delegation

The Collaborator orchestrates the workflow. It reads the request, checks the gates, and routes
the work. The Collaborator may run a command inline. It may delegate a command to a subagent with
the matching persona. Delegation is a judgement call. Base it on the task complexity and the risk.
A small, low-risk task needs no subagent.

Each command file carries a `delegation` hint to keep the main context clean:

- `subagent-preferred` — the command reads or writes much. Prefer a subagent when your agent
  supports it. Run inline if the task is small and low-risk.
- `inline-ok` — the command is light or is a human gate. Inline is fine.

The hint is a nudge, not a rule.

| Command | Persona |
|---|---|
| `init`, `define`, `status`, `upgrade` | Collaborator |
| `build`, `change` | Developer |
| `approve`, `verify`, `release` | Gatekeeper |
| `review-pre-verify` | Sr Tech Lead |

The persona definitions are in `agents/`. The human owns both gate decisions. The Gatekeeper
validates the gate and transmits the human decision. The Sr Tech Lead reviews the build (L only).

## Routing and Resume

- User intent selects the outcome. Protocol state selects the next command.
- Check the files and the gate markers before you route a request.
- Route a build-shaped request to the first incomplete command in the lifecycle.
- Keep an answer bound to the active command. Do not treat an answer as permission to run another
  command.
- Keep a review report unchanged after you check the verdict marker.
- Classify each finding as a blocker or a deferred item. Only a blocker goes back. A deferred item
  keeps the gate markers and continues.
- To resume a session, read `.oystro-oss/handover.yaml` and the last three `SLICE_LOG.md` entries.
  Then check the active phase and the git branch. Continue from the active task.

## Bugs and Change Requests

Read `.oystro-oss/commands/change.md`. The workflow uses one change record (`CHG-NNN.yaml`) and
focused evidence. It skips the design and tasks cycle. It requires human approval of the change
record before implementation.

## Global Policy: Ponytail (YAGNI)

All personas must apply the Ponytail YAGNI framework. Act as a pragmatic senior developer.

1. Reject an over-engineered design.
2. Do not add a dependency when a native platform API or a standard library can solve the problem.
3. Do not write an abstraction layer for a simple problem.

Reject a design or a PR that breaks these rules during `review-pre-verify`.

## Project Context

The project domain model lives in `CONSTITUTION.md`. It records the project type, the domain, the
actors, the user journeys, the domain events, the actions, the state, and the invariants. It also
records the domain readiness path.

Read the domain model before you write a spec. Keep every spec aligned with it.

## Local Secret Scan (Mandatory)

Secrets must not leave the machine. CI scanning finds a leak only after the push. The local hooks
prevent it.

- Set `git config core.hooksPath` to `githooks`. If it is not set, run `./scripts/setup-hooks.sh`.
- Do not use `--no-verify` on `git commit` or `git push`.
- Install `gitleaks` with your package manager: macOS `brew install gitleaks`; Windows
  `winget install Gitleaks.Gitleaks`; Linux `apt`, `dnf`, or `pacman`.
- Run `./scripts/scan-secrets.sh` before you finish. It scans the working tree, including the
  gitignored files.
