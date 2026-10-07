# ADR-0001: Conventional Commits, branch naming, and pull request titles

| | |
|---|---|
| **Status** | Proposed |
| **Date** | 2026-10-07 |

## Context

Many people and AI coding agents commit to SalesWorks-IT repositories. Without a shared format, commit history is hard to scan, changelogs and release notes can't be generated reliably, and reviewers spend time on wording. Each repository currently decides for itself, and many do not follow one.

The `supernova` repository adopted [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) with matching branch names and has used it successfully. This ADR lifts the parts of that decision that apply to every repository, so they are written down in one place that people and agents can both read.

This ADR is guidance for every repository. It does not by itself block a commit or a merge. See [Enforcement](#enforcement).

## Decision

Adopt **Conventional Commits 1.0.0** for commit messages, and align **branch names** and **pull request titles** to the same types.

### Commit message format

```text
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

- **Subject line:** imperative mood, lowercase after the colon, no trailing period, 72 characters or fewer.
- **Scope:** optional, encouraged when the change is localised (for example `auth` or `api`). A scope is lowercase letters, digits, dots, or hyphens. Each repository may list its own scopes in its `AGENTS.md`. Omit the scope for repository-wide changes.
- **Body:** explain *why*, not *what*, and only when the subject is not enough.
- **Breaking changes:** put `!` after the type or scope (`feat(api)!: …`), or add a `BREAKING CHANGE:` footer.

### Allowed types

| Type | Use for |
|------|---------|
| `feat` | A new capability |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `chore` | Maintenance with no change in production behaviour (dependencies, tooling) |
| `refactor` | A code change that neither fixes a bug nor adds a feature |
| `test` | Adding or updating tests only |
| `ci` | CI/CD workflow and pipeline changes |
| `build` | Build system or packaging changes |
| `perf` | A performance improvement |
| `revert` | Reverts a previous commit |

Do not invent new types. Changing this list means amending this ADR.

### Examples

```text
fix(auth): clear session before redirecting to login
docs: add onboarding steps to README
chore(deps): bump serilog to 4.2.0
feat(api)!: remove the v1 orders endpoint
```

### Branch names

A branch name is lowercase kebab-case and starts with a type prefix from the table above:

```text
<type>/<short-description>
<type>/<scope>-<short-description>
```

Use hyphens only, never underscores or spaces, and keep the name short. `feat/auth-biometric-toggle` and `fix/login-redirect` are good. `feature/biometrics` (wrong type), `fix_login` (underscores), and `upgrade-deps` (no type prefix) are not.

### Pull request titles

The pull request title follows the commit format above. When the pull request is squash-merged, the title becomes the commit message on the default branch, so it is the one that matters most. Reviewers may ask for a reword before merge.

### Merging

Prefer **squash merge** for short-lived branches, so each pull request lands as one conventional commit. Merge commits that GitHub generates, and `fixup!` or `squash!` commits made during development, are exempt from the format.

History that exists before a repository adopts this ADR is not rewritten.

### Enforcement

Adopting this ADR does not block anything on its own. Enforcement is a separate, per-repository choice:

| Layer | Mechanism | Status |
|-------|-----------|--------|
| **Guidance** | This ADR, the default `CONTRIBUTING.md`, and each repository's `AGENTS.md` | In place once accepted |
| **Pull request title check** | A shared reusable workflow in this repository that a repository calls from a short workflow of its own | Follow-up |
| **Commit message check** | A `commit-msg` hook, for repositories that commit directly to the default branch | Follow-up |
| **Organisation ruleset** | A commit message pattern rule applied across repositories | Depends on the GitHub plan; confirm before relying on it |

Repositories that push directly to their default branch are not covered by a pull request title check. A commit message hook or a ruleset is the only mechanism that reaches them.

### Adoption

Adoption is phased. A repository adopts this ADR from its next commit, and records any exception, such as a scope list or a different merge strategy, in its own `AGENTS.md`. Start with a few pilot repositories before widening.

## Consequences

### Positive

- Scannable, consistent history across repositories.
- Changelogs and release notes can be generated later.
- People and agents share one explicit contract.
- Branch names correlate with commit types.

### Negative

- A small learning cost for people who have not used Conventional Commits.
- Without enforcement the standard drifts, as it did before it was written down.
- Repositories that commit straight to the default branch need a hook or ruleset to be covered at all.

### Neutral

- Enforcement can be added later without revising this decision.
- Per-repository scope lists mean `git log` vocabulary differs slightly between repositories.

## References

- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)
- [ADR-0002: Trunk-based development](0002-trunk-based-development.md)
- Origin: ADR-0018 in the `supernova` repository
