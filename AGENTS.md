# Guidance for AI coding agents

This is the SalesWorks-IT organisation `.github` repository. It holds the default community health files that every repository in the organisation inherits when it has no copy of its own. Any tool (Claude Code, Copilot, Cursor, etc.) may read this file.

## Before you change anything

1. This repository is **public**. Never add secrets, credentials, customer data, internal hostnames, or anything confidential.
2. Every change goes through a pull request, and the `tech-leads` team must approve it (see [`.github/CODEOWNERS`](.github/CODEOWNERS)). Do not push to `main`.
3. A change here can affect every repository that inherits the file. Say in the pull request which repositories could be affected.

## What this repository does and does not do

| Inherited by other repositories | Not inherited, each repository needs its own |
|---------------------------------|-----------------------------------------------|
| `.github/PULL_REQUEST_TEMPLATE.md` | `AGENTS.md` (copy from [`agent-templates/AGENTS.md`](agent-templates/AGENTS.md)) |
| `.github/ISSUE_TEMPLATE/` forms | `CODEOWNERS` |
| `CONTRIBUTING.md` | `dependabot.yml` |
| | Labels and workflows |

A repository that has its own copy of a file uses its own copy. A repository with its own `ISSUE_TEMPLATE/` folder ignores the default forms entirely.

## Files

- [`CONTRIBUTING.md`](CONTRIBUTING.md): how to raise and review a pull request.
- [`agent-templates/AGENTS.md`](agent-templates/AGENTS.md): the starting point for a repository's own `AGENTS.md`.
