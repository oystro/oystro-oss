# Release v0.1.0

**Date**: 2026-09-15

## What's New

- Initial public release of **Oystro OSS** — the file-based software engineering protocol
  for AI coding assistants.
- Protocol reference only: conventions, templates, command contracts, and persona definitions.
- Runtime capabilities (enforcement, evidence, dashboards, memory/migrations, skill
  installation) are provided by the Oystro engine (paid offering), not by this repository.
- Added `MIGRATION.md` describing the upgrade of `.oystro-oss/` project state to `.oystro/`
  when adopting the paid product.
- Unified slice contract: `/oystro:define` produces a single `spec.yaml` (design and tasks
  internalised). Two human gates: `/oystro:approve` (before build) and `/oystro:release` (after
  verify). `/oystro:review-pre-verify` is an L-level-only agent review, not a human gate.

## Out of scope in this repository

This repository ships the protocol only. The following capabilities are intentionally absent
from the OSS repository and are provided by the Oystro engine (paid offering):

- enforcement, evidence capture, traceability, and blocked-state detection
- status dashboard and status server
- migration engine
- skill selection/installation
- release automation and pre-commit hooks
- runtime-specific templates
- legacy standalone contracts and their artifacts: `/oystro:design`, `/oystro:tasks`,
  `/oystro:review-pre-build`, the Sr Architect persona, and the standalone `design.md`/
  `tasks.md`/`spec.md` templates (superseded by `spec.yaml`)

## License

AGPL-3.0-only. See [LICENSE](LICENSE).
