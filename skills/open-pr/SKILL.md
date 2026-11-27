---
name: open-pr
description: Open a pull request in a SalesWorks-IT repository that follows org ADR-0001 (commits, branches, titles) and its ClickUp task ID rule. Use when the user asks to open, raise or create a PR, or to push work for review.
---

# Open a pull request

Read the repository's `AGENTS.md` first. If it conflicts with this skill, `AGENTS.md` wins.

## 1. Get the ClickUp task

- Ask the user for the ClickUp task ID and URL if they have not given them.
  Never invent an ID. If there is no ticket, stop and say one is needed first.
- Read the task name. The title and branch are written from it.

## 2. Check the branch

- Name: `<type>/<task-id>-<short-slug>`, lowercase kebab-case.
  Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`.
- If the current branch has a tool-generated name, rename it before pushing.
  Do not push to a shared branch.

## 3. Check the change

- Read the diff against the base. Stop and ask if it contains secrets,
  connection strings, customer data, or a weakened test, linter or CI step.
- Run the build, test and lint commands listed in the repository's `AGENTS.md`.
  Report failures with their output. Do not open the PR over a failing check.
- Flag high-risk files in the PR body: authentication, authorisation, personal
  data, database migrations, public APIs, CI or deployment pipelines,
  `AGENTS.md`, and `CODEOWNERS`. A tech lead must approve these.

## 4. Write the title

`<type>[(scope)][!]: <description> (<task-id>)`

- Imperative mood, lowercase after the colon, no trailing period.
- At most 72 characters including the task ID.
- Written from the task name, not copied verbatim.
- Example: `fix: clear analyzer warnings in core (z93r0bcg96)`

## 5. Write the body

1. First line: `ClickUp: [<exact task name>](<task url>)`
2. What changed, in two or three bullets.
3. Why, in one or two sentences.
4. Notes for the reviewer: high-risk files, and anything not verified or not run.
5. The AI attribution line the repository asks for.

## 6. Open it

- Base branch: the one the user or tech lead names. If unknown, ask.
  Do not guess between `DEV`, `release/*`, `hotfix/*` and the default branch.
- Confirm the title, base branch and body with the user before creating the PR.
- After creating it, report the PR URL and check that ClickUp shows the link.
