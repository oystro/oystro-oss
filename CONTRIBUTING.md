# Contributing to Oystro OSS

Thanks for your interest in contributing! This project is the open-source
core of **Oystro** — an Agentic Development Lifecycle (ADLC) protocol for
AI coding agents.

By contributing you agree to the terms below. Please read this document
fully before opening your first pull request.

---

## 1. Licensing — AGPL-3.0 + Contributor License Agreement

This repository is licensed under the **GNU Affero General Public License,
version 3** (see [`LICENSE`](./LICENSE)). Oystro
dual-licenses the core
commercially, so in order to accept contributions we require a **Contributor
License Agreement (CLA)**.

### The Contributor License Agreement

By submitting a contribution (code, documentation, tests, or any other
material), you agree to grant **Oystro** and its
successors a **perpetual, worldwide, non-exclusive, royalty-free,
irrevocable license** to
use, copy, modify, publish, distribute, sublicense, and create derivative
works from your contribution — including the right to license your
contribution under terms other than the AGPL (for example, within
proprietary commercial products) — **without additional compensation and
without seeking further permission from you.**

You represent that:

- You are legally entitled to grant the above license (you own the work, or
  you have the authority to contribute it on behalf of its owner); and
- Your contribution is your original work and does not infringe the rights
  of any third party.

### How to sign

1. Add a line to **every commit message**:
   ```
   Signed-off-by: Your Name <your@email.com>
   ```
2. On your first pull request, comment with:
   ```
   I agree to the Oystro OSS Contributor License Agreement.
   ```

The `Signed-off-by` line certifies the **Developer Certificate of Origin
(DCO)** — that you have the right to submit the contribution under the
project's license and CLA. Together, the DCO + CLA let us incorporate
community contributions into both the AGPL core and our commercial products.

---

## 2. Code of Conduct

Be respectful and constructive. Harassment, discrimination, or abusive
behavior of any kind will not be tolerated. By participating you agree to
maintain a professional and inclusive environment for everyone.

---

## 3. Getting Started

1. Fork the repository and clone it locally.
2. Create a feature branch: `git checkout -b feat/your-change`.
3. Make your changes and commit them (with the `Signed-off-by` line above).
4. Push and open a pull request against `main`.

---

## 4. Development Conventions

- **Workflow scope:** this repo implements the file-based Oystro workflow
  (`oystro:build`, `oystro:verify`, S/M/L levels, narrative log, git conventions).
  Commercial Oystro features are intentionally **not** in this repository.
- **Keep it file-based:** the open-source core is designed to work with any
  agent via plain files. Avoid adding dependencies on a specific agent,
  runtime, or vendor.
- **Tests:** add or update tests for any code change and run the existing
  suite before submitting.

### Scope and Direction

We keep `oystro-oss` deliberately small, and we intend to keep it that way. It
is the stable, file-based core: the conventions, the templates, the command
contracts, and the personas. A small surface is easier to read, easier to
audit, and easier to adopt with any agent.

So we are cautious about new surface area:

- **Welcome:** documentation, bug fixes, clarity improvements, and small,
  well-scoped changes.
- **Please discuss first:** a large feature. The runtime capabilities —
  deterministic enforcement, dashboards, memory, migrations, and team
  workflows — live in the **Oystro Standard edition**, the team's main focus.

If you are unsure where your idea fits, open a discussion before you build.
Describe the problem and the smallest change that solves it. We would rather
agree on the scope early than have you build something that does not fit.

---

## 5. Reporting Issues

- Search existing issues first to avoid duplicates.
- Provide a clear title, steps to reproduce, expected vs. actual behavior,
  and the environment/version you used.

---

## 6. Security

Please do **not** file a public issue for security vulnerabilities. Report
them privately to `security@oystro.com` so we can address them before
disclosure.

---

## 7. Questions

Open a discussion, or reach out to the maintainers. Thank you for helping
make Oystro OSS better!
