---
name: oystro-status
description: Show the current project state.

persona: Collaborator
delegation: inline-ok  # read-only
reason: Read-only operation
runtime: Oystro engine (status, JSON, and static HTML output)

gates:
  - check: .oystro-oss/ exists
    on_fail: STOP. Run oystro:init.

actions:
  - read: version.txt
  - read: .oystro-oss/plan.yaml (if it exists)
  - read: .oystro-oss/SLICE_LOG.md
  - scan: specs/active/*
  - scan: deferred.md in the active features
  - read: .oystro-oss/handover.yaml (if it exists; the Oystro engine writes it)
  - display_formatted_status_report: Show the version, the phase progress, the active specs, the deferred item counts, the recent commits, and the handover view.
  - render_json: Write the raw status JSON to stdout when --json is passed (Oystro engine).
  - render_html: Write the static HTML status to .oystro-oss/status/index.html when --html is passed (Oystro engine).

must_do:
  - Show the complete state snapshot.
  - Include the last 3 SLICE_LOG entries.
  - Support the JSON output with --json.
  - Support the self-contained static HTML output with --html.
  - Render the current derived handover state (Oystro runtime).

must_not_do:
  - Change anything.
  - Run a test or a build.
  - Make a network call.
  - Start a server.
  - Invoke an LLM review.
  - Open a subprocess that can download a dependency.
---
