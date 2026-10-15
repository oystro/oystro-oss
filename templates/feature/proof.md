---
name: proof
description: >
  The raw evidence for one slice. The agent writes this during oystro:verify. The human reviews it
  at Human Gate 2. Do not edit it by hand.
agent_contract:
  prerequisites:
    - id: PRE-001
      action: "The slice's evidence contract has run."
  outputs:
    - id: OUT-001
      path: "proof.md raw evidence"
---

# Proof — [FEATURE_ID]

**Feature**: [FID]
**Phase**: [PHASE_ID]
**Date**: [YYYY-MM-DD]

> One block per evidence scenario. Paste the raw command output. No summary, no paraphrase.

## SC-001 — [scenario]

- **Command**: `[command]`
- **Exit status**: [code]
- **Recorded at**: [ISO timestamp]

```
[raw stdout / stderr]
```

## SC-002 — [scenario]

- **Command**: `[command]`
- **Exit status**: [code]
- **Recorded at**: [ISO timestamp]

```
[raw stdout / stderr]
```

## Unit tests

- **Command**: `[test command]`
- **Exit status**: [code]

```
[raw test output]
```
