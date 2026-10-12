<!--
Template for a repository's own AGENTS.md. Copy it to the root of the repository,
replace every TODO, and delete this comment. GitHub does not inherit AGENTS.md from
the organisation .github repository, so each repository needs its own copy.
Keep it short and link to docs instead of repeating them.
-->

# Guidance for AI coding agents

This file is an IDE- and agent-agnostic entry point. Any tool (Claude Code, Copilot, Cursor, etc.) may read it when working in this repository.

## What this repository is

TODO: one or two sentences on what the repository does and who uses it.

## Before you change code

1. Read [`README.md`](README.md).
2. TODO: link the architecture or design docs, if any.
3. Follow the patterns in the surrounding code. Match its naming, comment density, and idiom.

## Commands

| Task | Command |
|------|---------|
| Build | TODO |
| Test | TODO |
| Lint / format | TODO |

Run the tests and the linter before you open a pull request.

## Code quality

The full rule is [ADR-0003](https://github.com/SalesWorks-IT/.github/blob/main/docs/adr/0003-code-quality-and-agentic-contributions.md).

- **Smallest correct diff.** Solve the stated problem only. No unrelated refactors, renames, or speculative features. Split a large change.
- **Terse and readable.** Short functions, clear names, dead code deleted. No helper or class used only once.
- **Match the existing code.** Read the surrounding code first. Follow its naming, imports, and error handling.
- **Comments say why, not what.** No banners and no running commentary on obvious code.
- **No new dependency** without a justification in the pull request.
- **Handle failure paths,** not only the happy path. Never swallow an error silently.
- **Tests assert intended behaviour,** not whatever the code does. Update them when behaviour changes.
- **Never weaken a check.** Do not disable, skip, or relax a test, linter, or CI step to make a change pass.
- **High-risk changes need a human.** Authentication, authorisation, personal data, database migrations, public APIs, CI and deployment pipelines, `AGENTS.md`, and `CODEOWNERS`: draft if asked, but a tech lead approves.
- **Say when AI wrote it** (recommended): a `Co-Authored-By` trailer or a line in the pull request description.

## Commits and branches

Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). The full rule is [ADR-0001](https://github.com/SalesWorks-IT/.github/blob/main/docs/adr/0001-conventional-commits-and-branch-naming.md).

- **Commit and pull request title:** `<type>[(scope)][!]: <description>`. Imperative mood, lowercase after the colon, no trailing period, 72 characters or fewer.
- **Types:** `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`. Do not invent others. If a change fits two, use the one that matters most to a reader of the history.
- **Scope:** optional, lowercase. TODO: list this repository's scopes, or delete this line.
- **Breaking change:** put `!` after the type or scope, or add a `BREAKING CHANGE:` footer.
- **Branch name:** lowercase kebab-case, `<type>/<short-description>`, for example `fix/login-redirect`.
- **Merging:** prefer squash merge, so the pull request title becomes the commit message.
- Merge commits, `git revert` messages, and `fixup!` or `squash!` commits are exempt.
- Example: `fix(auth): clear the session before redirecting to login`.

## Pull requests

- Work on a branch and open a pull request. Do not push to the default branch.
- Follow the organisation's [contributing guide](https://github.com/SalesWorks-IT/.github/blob/main/CONTRIBUTING.md).
- Keep a change focused on one thing, with a description that says what changed and why.

## Never

- Commit secrets, credentials, connection strings, or customer data.
- Disable or weaken tests, linters, or CI to make a change pass.
- Edit this file, `CODEOWNERS`, or CI workflows without review from a tech lead.

## Repository-specific notes

TODO: anything an agent cannot learn from the code, for example required environment setup, generated files that must not be edited, or areas that need extra care.
