---
name: open-pr
description: Take finished work to a pull request in a SalesWorks-IT repository, following org ADR-0001 (commits, branches, titles) and its ClickUp task ID rule. Creates the branch, commit and, if the work has no ClickUp item yet, the item itself. Use when the user asks to open, raise or create a PR, or to push work for review.
---

# Open a pull request

The engineer should not have to branch, commit or create a ClickUp item by hand.
Do those steps yourself, and ask only the questions marked **Ask**.
Read the repository's `AGENTS.md` first. If it conflicts with this skill,
`AGENTS.md` wins.

## 1. See where you are

- Run `git status`, and read the current branch and the diff against the base.
- If the changes are on the default branch or another shared branch, create a
  new branch for them in step 3. Never commit to a shared branch.
- If there are uncommitted changes that are not part of this task, leave them
  out and say so.

## 2. Find or create the ClickUp task

Where the repository tracks work in ClickUp (org ADR-0001, "Task IDs"). Skip
this step and the task ID elsewhere if it does not.

1. **The user gave a task ID or URL.** Read the task. Its name is the source
   for the title and branch slug.
2. **No ID given.** Search ClickUp for an open item that matches the change.
   **Ask** the user to confirm a match before using it.
3. **No match.** Create one, so the PR has a real ID. **Ask once** to confirm:
   - Name: a plain-English outcome, not a commit message.
   - List: the one the user names. If unknown, ask. Do not guess.
   - Type: Bug for a `fix`, otherwise Task, unless the user says otherwise.
   - Assignee: the engineer.
   - Description: what changed and why, taken from the diff.
   Then create it and use the ID ClickUp returns.
4. **No ClickUp access.** Say so and ask for an ID, or for the engineer to
   create the item. Never invent an ID.

Do not change the task's status unless the user asks. Statuses are the team's
convention.

## 3. Create or rename the branch

- Name: `<type>/<task-id>-<short-slug>`, lowercase letters, digits and hyphens
  only, with one `/` after the type. Lowercase the task ID.
  Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`.
- Work may have started on a branch with a temporary or tool-generated name.
  Rename it before pushing (`git branch -m`).
- Branch from the base branch the user or tech lead names. If unknown, **ask**.
  Do not guess between `DEV`, `release/*`, `hotfix/*` and the default branch.

## 4. Check the change

- Stop and ask if the diff contains secrets, connection strings, customer data,
  or a weakened test, linter or CI step.
- When the change touches code they cover, run the build, test and lint
  commands listed in the repository's `AGENTS.md`. Report failures with their
  output. Do not open the PR over a failing check.
- Flag high-risk files in the PR body: authentication, authorisation,
  cryptography, personal data, database migrations, public APIs, CI or
  deployment pipelines, `AGENTS.md`, and `CODEOWNERS`. A tech lead must
  approve these.

## 5. Commit

- Commit messages follow the title format below. For a squash merge the PR
  title becomes the commit message, so the task ID belongs in the title.
- Say when AI wrote it (recommended by org ADR-0003): a `Co-Authored-By`
  trailer on the commit.

## 6. Write the title

`<type>[(scope)][!]: <description> (<task-id>)`

- Imperative mood, lowercase after the colon, no trailing period.
- At most 72 characters including the task ID.
- Written from the task name, not copied verbatim.
- Example: `fix: clear analyzer warnings in core (z93r0bcg96)`

## 7. Write the body

Use the repository's pull request template (`.github/PULL_REQUEST_TEMPLATE.md`,
or the org default). `gh pr create --body` skips it, so copy its sections in.
Fill every section: Summary, Risk and rollback, Testing notes, Checklist, Related.

- First line, above the first heading: `ClickUp: [<exact task name>](<task url>)`
- Risk and rollback names any high-risk area from step 4.
- Testing notes say what was run and what was not.

## 8. Open it, then link it back

- **Ask** to confirm the title, base branch and body, then push and create the PR.
- Add a comment on the ClickUp task with the PR URL, and check that ClickUp
  shows the PR link.
- If a PR already exists without a task ID: create the task as in step 2, edit
  the PR title to add the ID, and add the ClickUp line to the body. Leave the
  branch name; it cannot be renamed once the PR is open.
