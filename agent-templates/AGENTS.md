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
