---
name: oystro-status
description: Display current project state

persona: Manager
reason: Read-only operation
runtime: Oystro engine (status, JSON, and static HTML output)

gates:
  - check: .oystro-oss/ exists
    on_fail: STOP, run /oystro:init

actions:
  - read: version.txt
  - read: .oystro-oss/PHASES.md
  - read: .oystro-oss/SLICE_LOG.md
  - scan: specs/active/*
  - scan: deferred.md in active features
  - read: .oystro-oss/handover.yaml (if exists; written by the Oystro engine; `.oystro/` after migration)
  - display_formatted_status_report: version, phases progress, active specs, deferred item counts, recent commits, handover view
  - render_json: output raw status JSON to stdout if --json is passed (Oystro engine)
  - render_html: write static HTML status to .oystro-oss/status/index.html if --html is passed (Oystro engine)

must_do:
  - Show complete state snapshot
  - Include last 3 SLICE_LOG entries
  - Support JSON output via --json
  - Support self-contained static HTML output via --html
  - Render current derived handover state (Oystro runtime)

must_not_do:
  - Modify anything
  - Run tests or a build
  - Make network calls
  - Start a server
  - Invoke LLM review
  - Open a subprocess that might download dependencies
---
<!-- Reference contract. Runtime execution and enforcement are provided by the Oystro engine (paid offering). -->
