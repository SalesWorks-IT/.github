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
- **A release is a tag** on the default branch. See [Releases and tags](#releases-and-tags).
- **A hotfix is a short branch** from that tag, merged back to the default branch.
- **A pull request is the review.** It is not a place to park work.

Long-lived **environment branches** such as `develop`, `FT/main`, `UAT/main`, and per-phase branches are replaced by deploying the same commit through the environments, driven by the pipeline and by tags. A repository that genuinely needs a long-lived branch records the reason and the branch in its own `AGENTS.md`. That is an exception, not the default.

The default branch name is not part of this decision. Renaming `master` to `main` is optional and separate.

### Releases and tags

A release is an **annotated tag** named `v<major>.<minor>.<patch>`, for example `v1.12.0`.

- **Version numbers** follow [Semantic Versioning](https://semver.org/): a breaking change raises the major number, a new capability the minor number, and a fix the patch number. A person chooses the number for now. Once Conventional Commits are in common use, a tool can derive it from the commit types in [ADR-0001](0001-conventional-commits-and-branch-naming.md).
- **A tag marks the exact commit that shipped.** Do not move or reuse a tag. A fix means a new version.
- **Each release has a GitHub Release** created from its tag, with generated notes. The notes list pull request titles, so they read well when titles follow ADR-0001.
- **Until a repository adopts the single trunk,** its `release/<version>` branch continues to exist, and the tag goes on that branch's tip at release time. The tag keeps its `v` prefix even when the branch name has none.

#### Release candidates (optional)

When a release goes through UAT, name what is in UAT with a release candidate tag: `v<major>.<minor>.<patch>-rc.<N>`, for example `v1.12.0-rc.1`.

- Tag the commit that is sent to UAT, and mark its GitHub Release as a **pre-release**.
- If UAT passes, put the final `v1.12.0` tag on the **same commit** as that candidate. What was tested is what ships, with no new commits in between.
- If a fix is needed, merge it and tag a new candidate (`-rc.2`). Do not edit or move an earlier candidate.
- Candidates are recommended when a release goes through UAT, and optional otherwise. A hotfix can skip them. They are never created per merge.
- A candidate tag replaces an environment branch such as `UAT/main` as the way to say what is in UAT.

#### Existing tags

As of October 2026, NovaAPI, NovaCRMWebApp, and NovaCRMApi each have a `v1.12.0` tag, which matches this format. Earlier releases have no tag and are not tagged retroactively unless someone needs them.

#### Later

Once the single trunk is in place, a tool can create tags and releases from the commit types on each merge. That is a separate decision, because it needs reliable Conventional Commits and a pipeline that deploys from tags or commits.

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
- A tag, not a branch, says what is in UAT or production, and a hotfix has a known starting point.

### Negative

- Work that runs longer than a day or two has to be split or hidden, not parked on a branch.
- Repositories that already use `develop` or `release/*` need a migration plan or a recorded exception.
- Someone has to create the tags and releases until a tool does, and the pipeline must be able to build from a tag or a commit.

### Neutral

- This ADR says nothing about how a repository protects its default branch. That is a separate decision.

## References

- [ADR-0001: Conventional Commits, branch naming, and pull request titles](0001-conventional-commits-and-branch-naming.md)
- [Semantic Versioning 2.0.0](https://semver.org/)
- Origin: ADR-0018 in the `supernova` repository
