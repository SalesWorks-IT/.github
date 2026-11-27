---
name: open-pr
description: Take finished work to a pull request in a SalesWorks-IT repository, following org ADR-0001 (commits, branches, titles) and its ClickUp task ID rule. Creates the branch, commit and, if the work has no ClickUp item yet, the item itself. Use when the user asks to open, raise or create a PR, or to push work for review.
---

# Open a pull request

The engineer should not have to branch, commit or create a ClickUp item by hand.
Do those steps yourself, and ask only the questions marked **Ask**.
Read the repository's `AGENTS.md` first. If it conflicts with this skill,
`AGENTS.md` wins.

**Order:** commit locally, ask for confirmation, then push and create the PR.
Do not push before the confirmation in step 8.

## 1. See where you are

- Run `git status` and read the diff against the default branch. The real base
  branch is settled in step 3.
- Check whether a PR already exists for the current branch
  (`gh pr list --head <branch>`). If one does, see the last bullet of step 8.
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
3. **No match.** Create one, so the PR has a real ID. Put every missing detail
   into **one** question:
   - Name: a short plain-English outcome, not a commit message, for example
     "Farewell handles an empty name".
   - List: the one the user names. If unknown, ask. Do not guess.
   - Type: Bug for a `fix`, otherwise Task, unless the user says otherwise.
   - Assignee: the engineer. Use the ClickUp user "me", or `git config user.email`
     to find them. If you cannot tell, ask.
   - Description: what changed and why, taken from the diff.
   Then create it and use the ID ClickUp returns.
4. **No ClickUp access, or the engineer has no ID and will not create one.**
   Never invent an ID. No task, no PR (org ADR-0001): commit locally on a
   branch named `<type>/<short-slug>`, do **not** push, and do **not** open the
   PR. Tell the engineer what is ready and that a task ID is the only thing
   missing.

Do not change the task's status unless the user asks. Statuses are the team's
convention.

## 3. Create or rename the branch

- Name: `<type>/<task-id>-<short-slug>`, lowercase letters, digits and hyphens
  only, with one `/` after the type. Lowercase the task ID. Keep the slug to
  about five words.
  Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`.
- Work may have started on a branch with a temporary or tool-generated name.
  Rename it (`git branch -m`) before pushing.
- Branch from the base branch the user or tech lead names, or the one
  `AGENTS.md` names. If neither, **ask**. Do not guess between `DEV`,
  `release/*`, `hotfix/*` and the default branch.

## 4. Check the change

- Stop and ask if the diff contains secrets, connection strings, customer data,
  or a weakened test, linter or CI step.
- When the change touches code they cover, run the build, test and lint
  commands listed in the repository's `AGENTS.md`. Report failures with their
  output. Do not open the PR over a failing check. If a repository lists no
  linter, say "not run, none configured" in the Testing notes.
- Flag high-risk files in the PR body: authentication, authorisation,
  cryptography, personal data, database migrations, public APIs, CI or
  deployment pipelines, `AGENTS.md`, and `CODEOWNERS`. A tech lead must
  approve these.

## 5. Commit locally

- Commit messages follow the title format in step 6. A single-commit branch
  uses the PR title as its subject, task ID included. On a branch with several
  commits, only the PR title needs the ID, because the squash merge uses it.
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
- Checklist: tick a box only when it is true and you checked it. Leave the
  others unticked with a short reason, for example "no linter configured".
  Never tick the squash-merge box; that is for whoever merges.
- Related: `Related to <ClickUp task URL>`. Use "Closes" only for an issue
  that closes when the PR merges into the default branch.
- Add a line saying AI wrote it (ADR-0003 recommends this), unless the
  repository's `AGENTS.md` says otherwise.

## 8. Confirm, push, open it, then link it back

- **Ask** to confirm the title, base branch and body. If the engineer changes
  the title, amend a single commit's message to match before pushing.
- Push the branch, then create the PR with `gh pr create`. Take the PR URL
  from its output.
- Add a comment on the ClickUp task with the PR URL. Check that ClickUp shows
  the PR link. If it does not appear after a few minutes, tell the engineer;
  do not retry.
- If a PR already exists without a task ID: create the task as in step 2, edit
  the PR title to add the ID, and add the ClickUp line to the body. Leave the
  branch name; it cannot be renamed once the PR is open.
