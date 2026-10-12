# ADR-0003: Code quality and agentic contributions

| | |
|---|---|
| **Status** | Proposed |
| **Date** | 2026-10-12 |

## Context

People and AI coding agents both change SalesWorks-IT repositories. Mechanical quality varies by repository: some run linters and tests in CI and some do not. None of that says what a good change looks like: how large it should be, how terse or readable the code should be, when to add an abstraction, or how a reviewer should treat an agent-written diff.

Without a shared contract, agents tend towards large diffs, verbose comments, speculative features, and quiet weakening of checks to make a change pass. Reviewers have no documented basis for pushing back.

This ADR applies to **all** contributions, human and agent. It builds on [ADR-0001](0001-conventional-commits-and-branch-naming.md), which sets the commit, branch, and pull request title format, and must not weaken any CI, lint, or review rule a repository already has. It is adapted from ADR-0033 in the `supernova` repository, minus the parts specific to that project's stack.

## Decision

Adopt the following **code quality contract**. People and agents follow it when writing or reviewing a change.

### 1. Smallest correct diff

- Solve the stated problem only. Do not refactor unrelated files, rename for taste, or add speculative features.
- Prefer extending existing types and functions over a parallel implementation.
- If a change needs a large diff, split it into reviewable pull requests or justify the size in the description.

### 2. Terse and readable

- **Terse:** short functions, clear names, dead code deleted, no redundant comments or boilerplate.
- **Readable:** a reviewer should grasp the intent without tracing deep indirection. Avoid clever one-liners, deep nesting, and opaque abbreviations.
- Logic used once stays inline. Do not add a helper or class that is used once unless it clearly improves clarity.

### 3. Match the existing code

- Read the surrounding code before editing. Match its naming, imports, error handling, and documentation level.
- Follow the repository's own structure and the patterns its `AGENTS.md` points to.

### 4. Comments

- Comment **why**: a non-obvious business rule, a platform quirk, a security reason. Do not narrate what the next line obviously does.
- No banner blocks and no agent-written running commentary on self-explanatory code.

### 5. Dependencies and surface area

- No new package without a clear justification in the pull request.
- Keep public APIs small. Prefer private helpers unless reuse is real.

### 6. Errors and edge cases

- Handle failure paths: I/O, authentication, permissions, cancellation, and empty or null input, not only the happy path.
- Do not swallow errors silently. An empty `catch` needs a logged or visible outcome, unless an explicit rule says otherwise.

### 7. Tests

- Add or update tests when behaviour changes. A test asserts **intended** behaviour, not whatever the implementation happens to do.
- Treat a large test rewrite as a signal: reviewers confirm the assertions still match what the product should do.
- Avoid tests that only repeat the implementation.

### 8. Pull requests

- The title follows [ADR-0001](0001-conventional-commits-and-branch-naming.md). The description says **what** changed and **why**.
- Run the repository's tests and linters before opening a pull request, when the change touches code they cover.
- Do not disable, skip, or relax a test, linter, or CI check to make a change pass.

### 9. Secrets and data

- Never commit secrets, credentials, connection strings, or customer data, including in tests, fixtures, configuration files, and history.
- If one is committed by mistake, tell a tech lead straight away so it can be rotated. Deleting it in a later commit does not remove it from history.

### 10. Review depth by risk

| Tier | Examples | Expectation |
|------|----------|-------------|
| **Low** | Documentation, copy, isolated UI tweaks, tests for unchanged behaviour | Checks green. Standard review. |
| **Medium** | Business logic, integrations, new endpoints or services | Checks green. Failure-path tests where relevant. A reviewer who knows the area. |
| **High** | Authentication, authorisation, cryptography, personal data, database migrations, public APIs, CI and deployment pipelines, `AGENTS.md`, `CODEOWNERS` | A human design review. An agent may draft the change, and a tech lead approves it. |

Automated tools may point out problems. They do not replace a human judging a high-tier change.

### Disclosure of AI-assisted work (recommended)

Reviewers calibrate their attention to how a change was written. When an AI tool wrote a substantial part of a commit, say so, either with a `Co-Authored-By: <tool>` trailer on the commit or a line in the pull request description. This is **recommended, not required**. It does not change who is accountable: the person who opens or merges the pull request is, as [ADR-0001](0001-conventional-commits-and-branch-naming.md#ai-authored-changes) says.

**Tech leads should confirm whether to keep this as a recommendation, make it a requirement, or drop it.**

### Enforcement

This ADR is guidance. It is carried by each repository's `AGENTS.md` and applied in review. Nothing in this ADR blocks a merge. Where a repository has a `CODEOWNERS` file, the high tier maps naturally onto it.

## Consequences

### Positive

- A shared, written bar for maintainable code beyond "the linter is clean".
- Reviewers can reject scope creep and verbosity with a documented basis.
- Agents get the same rules people do, in a form they can follow.

### Negative

- Several rules are judgment calls, so reviewers must apply them consistently.
- It does not enforce terseness or scope by automation.
- Repositories that already have `AGENTS.md` need a small update to pick up the summary.

### Neutral

- A stack-specific ADR, for example for .NET or Angular, can add detail without changing this one.
- Review depth cannot be enforced on a private repository until the plan allows branch protection.

## References

- [ADR-0001: Conventional Commits, branch naming, and pull request titles](0001-conventional-commits-and-branch-naming.md)
- [ADR-0002: Trunk-based development](0002-trunk-based-development.md)
- Origin: ADR-0033 in the `supernova` repository
