# Contributing

These are the default contribution notes for repositories in the SalesWorks-IT organisation. A repository can override them with its own `CONTRIBUTING.md`.

## Raising a pull request

1. Create a branch from the default branch. Do not push directly to it.
2. Keep the change focused on one thing.
3. Describe what changed and why. The pull request template prompts for this.
4. Make sure the tests and any linters pass before you ask for a review.
5. Ask for a review from someone who knows the area. Address comments with new commits rather than force-pushing over reviewed work.

## Reviews

- Reviewers check correctness, tests, and whether the change fits the surrounding code.
- Repositories with a `CODEOWNERS` file need an approval from a listed owner.
- Do not merge your own change without a review unless the repository's rules say you may.

## Secrets and confidential data

Never commit secrets, credentials, connection strings, or customer data, including in tests, fixtures, and history. If you commit one by mistake, tell a tech lead straight away so it can be rotated. Removing it in a later commit does not remove it from history.

## Working with AI coding agents

Repositories should have an `AGENTS.md` at the root. Start from [`agent-templates/AGENTS.md`](agent-templates/AGENTS.md). The same review rules apply to agent-authored changes as to any other change.
