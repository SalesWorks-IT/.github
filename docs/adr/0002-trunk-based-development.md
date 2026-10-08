# ADR-0002: Trunk-based development

| | |
|---|---|
| **Status** | Proposed |
| **Date** | 2026-10-07 |

## Context

Long-lived branches such as `develop` and `release/*` let work sit unreviewed, drift from the default branch, and merge late and painfully. The `supernova` repository uses trunk-based development and keeps the default branch releasable.

Today the repositories do not share a branching model. A survey of four of them in October 2026 found `phaseN` branches, a `DEV` branch with `release/*`, and `develop` plus `FT/main` and `UAT/main` environment branches, and the default branch is `master`. Promoting a change means merging it between long-lived branches, which produces many merge commits and makes it hard to tell what is deployed where.

This is the most opinionated decision in this set and the hardest to adopt, because it changes how releases move through environments. **Tech leads should confirm that this default fits before it is accepted, and each repository adopts it on its own timetable.**

## Decision

The default branch is the **trunk**. Repositories integrate work into it in small, reviewed pieces.

- **The default branch is the only long-lived branch.** There is no `develop` branch and no standing `release/*` branch.
- **Other branches are short-lived.** They live for a day or two, merge, and are deleted.
- **A change lands in small pieces.** Unfinished work stays off the default branch, or is hidden behind a flag, so the default branch stays releasable.
- **A release is a tag** on the default branch.
- **A hotfix is a short branch** from that tag, merged back to the default branch.
- **A pull request is the review.** It is not a place to park work.

Long-lived **environment branches** such as `develop`, `FT/main`, `UAT/main`, and per-phase branches are replaced by deploying the same commit through the environments, driven by the pipeline and by tags. A repository that genuinely needs a long-lived branch records the reason and the branch in its own `AGENTS.md`. That is an exception, not the default.

The default branch name is not part of this decision. Renaming `master` to `main` is optional and separate.

### Adoption

Adoption is per repository and comes after [ADR-0001](0001-conventional-commits-and-branch-naming.md) is in use. A repository planning the move should:

1. List its current long-lived branches and what each is used for.
2. Decide how each environment gets a build, for example from a tag or from the default branch.
3. Move the pipeline first, then retire the old branches once nothing deploys from them.

Branch and commit naming follow [ADR-0001](0001-conventional-commits-and-branch-naming.md).

## Consequences

### Positive

- The default branch stays releasable.
- Smaller changes are easier to review and revert.
- Fewer merge conflicts, because branches do not drift for weeks.

### Negative

- Work that runs longer than a day or two has to be split or hidden, not parked on a branch.
- Repositories that already use `develop` or `release/*` need a migration plan or a recorded exception.

### Neutral

- This ADR says nothing about how a repository protects its default branch. That is a separate decision.

## References

- [ADR-0001: Conventional Commits, branch naming, and pull request titles](0001-conventional-commits-and-branch-naming.md)
- Origin: ADR-0018 in the `supernova` repository
