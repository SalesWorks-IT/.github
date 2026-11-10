# SalesWorks-IT `.github`

This is the organisation's shared engineering repository. It holds the defaults, standards, and templates that apply across SalesWorks-IT repositories, in one place that people and AI coding agents can both read.

## What is a `.github` repository?

GitHub treats a repository named exactly `.github` in an organisation as special. Some files placed here act as **defaults**: any repository in the organisation that has no copy of its own uses the one from here. Everything else in this repository is shared material that repositories link to or copy.

## Why we use it

- **Consistency.** Pull requests, issues, and contribution notes look the same everywhere, so people moving between repositories do not relearn the process.
- **One source for decisions.** Rules such as the commit format live as written, numbered decisions (ADRs) with their reasoning, not in scattered chat threads.
- **Agents follow the same rules as people.** AI coding agents read a repository's `AGENTS.md`. This repository holds the template for it and the standards it points to.
- **Controlled change.** A change here can affect every repository, so every change is a pull request that the `tech-leads` team approves.

## What is in it

| Path | What it is | How repositories get it |
|------|------------|-------------------------|
| [`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md) | Default pull request template | Inherited automatically |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/) | Default bug and feature issue forms | Inherited automatically |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How to raise and review a pull request | Inherited automatically |
| [`.github/workflows/pr-title.yml`](.github/workflows/pr-title.yml) | Reusable workflow that checks the pull request title and warns on the branch name | Opt in: a repository calls it (see below) |
| [`agent-templates/AGENTS.md`](agent-templates/AGENTS.md) | Starting point for a repository's own `AGENTS.md` | Copy it into the repository |
| [`docs/adr/`](docs/adr/README.md) | Organisation-wide decisions (ADRs) and their status | Read and link to them |
| [`AGENTS.md`](AGENTS.md) | Guidance for agents working on this repository itself | This repository only |
| [`.github/CODEOWNERS`](.github/CODEOWNERS) | Makes `tech-leads` the required reviewers here | This repository only |

A repository that has its own copy of a default file keeps using its own. A repository with its own `ISSUE_TEMPLATE/` folder ignores the default forms entirely.

### What is not inherited

GitHub does not share these from here, so each repository needs its own: `AGENTS.md`, `CODEOWNERS`, `dependabot.yml`, labels, and workflows. The reusable workflow in this repository only runs where a repository calls it.

## Using it in a repository

**Defaults.** Nothing to do. To adopt a default, do not keep your own copy of that file.

**The pull request title and branch name check.** Add this file as `.github/workflows/pr-title.yml` in your repository:

```yaml
name: PR title

on:
  pull_request:
    types: [opened, edited, synchronize, reopened]

permissions: {}

jobs:
  pr-title:
    uses: SalesWorks-IT/.github/.github/workflows/pr-title.yml@main
```

The title check reports pass or fail. The branch name check only warns, because a branch cannot be renamed once its pull request is open. Neither is a required check, so neither blocks a merge. The workflow only runs for pull requests whose target branch contains the file.

**`AGENTS.md`.** Copy [`agent-templates/AGENTS.md`](agent-templates/AGENTS.md) to the root of your repository, replace every `TODO`, and delete the comment at the top.

## Standards

The decisions are in [`docs/adr/`](docs/adr/README.md). The index there shows each one's status.

- [ADR-0001](docs/adr/0001-conventional-commits-and-branch-naming.md): Conventional Commits, branch names, and pull request titles. It applies to AI-authored work as well.
- [ADR-0002](docs/adr/0002-trunk-based-development.md): trunk-based development, release tags, and optional release candidates.
- [ADR-0003](docs/adr/0003-code-quality-and-agentic-contributions.md): code quality and agentic contributions.

These are guidance. The checks above report without blocking. Enforcement is a separate, later step and is described in the ADRs.

## Changing this repository

- Every change is a pull request. The `Protect main` ruleset requires an approval from the `tech-leads` team, and the repository allows **squash merge** only.
- Follow [ADR-0001](docs/adr/0001-conventional-commits-and-branch-naming.md) for the branch name and the pull request title.
- Say in the description which repositories the change could affect.
- **This repository is public.** Never add secrets, credentials, internal hostnames, customer data, or commercial or incentive details.
- New decisions start from [`docs/adr/template.md`](docs/adr/template.md).
