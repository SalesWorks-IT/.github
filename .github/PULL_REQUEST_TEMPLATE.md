## Summary

<!-- What does this change, and why? One short paragraph, then one bullet per problem fixed:
- **The problem, in bold.** What was wrong, what changed, and why this way. -->

## Risk and rollback

<!-- What could break, and for whom? How do we undo it: revert, feature flag, config?
Say if it touches a high-risk area (ADR-0003): auth, personal data, migrations, public APIs, CI, AGENTS.md, CODEOWNERS. -->

## Testing notes

<!-- How did you check it? Commands run and their results, tests added, manual steps.
For manual checks, a table helps:

| Case | Result |
| :--- | :--- |
| 1. Scenario: expected outcome | Pass / Fail / Not run (why) |

Say what you did NOT test. If something fails on the default branch too, say so and link its issue. -->

## Checklist

- [ ] The change is focused on one thing
- [ ] Tests and linters pass
- [ ] Docs are updated if behaviour changed
- [ ] No secrets, credentials, or customer data are included
- [ ] Merge with **Squash and merge**, unless this promotes one long-lived branch into another

## Related

<!-- Link the issue or ticket, e.g. Closes #123.
"Closes" only closes the issue when this merges into the default branch. Into any other branch, write "Related to #123". -->
