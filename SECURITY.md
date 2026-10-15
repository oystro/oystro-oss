# Security Policy

## Reporting a vulnerability

Do not open a public issue for a security vulnerability.

Report privately, either way:

- **GitHub private advisory** (preferred): [Report a vulnerability](https://github.com/oystro/oystro-oss/security/advisories/new)
- **Email**: security@oystro.com

Include what you can: the affected file, command, or template; the impact you believe it has; the
steps to reproduce; and any suggested fix.

We will acknowledge your report as soon as we can, investigate, and keep you updated. Please give us
a reasonable opportunity to release a fix before any public disclosure, and we will credit you in the
advisory unless you ask us not to.

## Scope

This repository is the **protocol**: conventions, templates, command contracts, personas,
and documentation. In scope:

- Guidance that could cause an agent to leak secrets, credentials, or private data.
- Templates or commands that could be induced to execute untrusted instructions, or to write outside
  the intended project boundary.
- Documentation that describes an unsafe practice as if it were the sanctioned workflow.

Out of scope for this repository:

- Vulnerabilities in third-party agents, models, or tools you drive with the protocol. Report those
  to their maintainers.
- The Oystro runtime (enforced gates, evidence capture, dashboards, memory). Report those through the
  same private channel and we will route them appropriately.
- Reports that require an already-compromised machine or an attacker with write access to your
  project.

## Supported versions

Oystro OSS is the protocol. Fixes land on `main` and are published as releases; use the latest
release. There are no long-term support branches.
