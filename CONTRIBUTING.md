# Contributing to Road-Martt

**Road-Martt** is the Shopify-powered dropshipping storefront in the broader Fleet Rx portfolio. Keep storefront commerce concerns—catalog presentation, customer journeys, and Shopify-backed operations—separate from the Fleet Rx marketing and private operations platform. This guide gives every contributor one secure, repeatable way to access the repository, create changes, and propose them for review.

> **Principle:** Never commit account credentials, API tokens, customer data, private order data, or production configuration. Use GitHub authentication for repository access and approved secret storage for application credentials.[1]

## Local Setup and Git Credentials

Use GitHub CLI with the system credential helper whenever possible. This avoids placing a personal access token in a repository URL, shell history, or tracked configuration. GitHub documents the browser-based login flow and its credential-helper integration for Git operations.[2] [3]

| Task | Command | Expected result |
|---|---|---|
| Authenticate securely | `gh auth login` | Follow the prompts, select GitHub.com, HTTPS, and browser authentication. |
| Confirm the active account | `gh auth status` | Verify that the expected GitHub account is active. |
| Configure Git to use the GitHub credential helper | `gh auth setup-git` | Git can authenticate to GitHub without embedding a token in `origin`. |
| Identify your commits | `git config --global user.name "Your Name"`<br>`git config --global user.email "you@example.com"` | Commits use your recognizable author identity. |
| Clone Road-Martt | `gh repo clone fleetrxi95-ctrl/Road-Martt` | Creates a local project directory with `origin` set to the repository. |

After cloning, enter the repository and verify that the remote uses an ordinary HTTPS URL. A remote URL should identify the repository, not contain a password or token.

```bash
cd Road-Martt
git remote -v
git status
```

If your organization requires a personal access token instead of browser sign-in, create a fine-grained token limited to the required repository and permissions, store it in GitHub CLI or a credential manager, and never paste it into a file or command that will be committed.[4]

## Working Safely

Begin each change from an up-to-date default branch. Use one short-lived branch for one coherent outcome, whether that outcome is documentation, a storefront component, a Shopify integration adjustment, or a test. Fork the repository when you do not have write access; GitHub’s fork workflow is the preferred collaboration path for external contributors.[5]

```bash
git switch main
git pull --ff-only origin main
git switch -c docs/improve-storefront-setup
```

Before editing, check the repository’s current README, open issues, and existing pull requests. When the project adds a framework or storefront implementation, use the documented install, lint, build, and test commands rather than inferring commands from another project.

## Commit Conventions

Write focused commits in the imperative mood. Each commit should answer one question: **what change is being made?** Use the following lightweight Conventional Commit pattern to keep history searchable and release notes understandable.

```text
<type>: <concise action>
```

| Type | Use it for | Example |
|---|---|---|
| `feat` | A new customer-facing or contributor-facing capability | `feat: add Road-Martt collection navigation` |
| `fix` | A defect correction | `fix: preserve cart quantity after refresh` |
| `docs` | README, onboarding, or architectural documentation | `docs: add Road-Martt contributor guide` |
| `style` | Visual or formatting changes with no behavior change | `style: align mobile product cards` |
| `refactor` | Internal restructuring without changing behavior | `refactor: isolate storefront data adapter` |
| `test` | Added or adjusted automated coverage | `test: cover cart total formatting` |
| `chore` | Tooling, housekeeping, or dependency maintenance | `chore: update local development instructions` |

Keep the summary under roughly 72 characters, avoid ending it with a period, and include a body when reviewers need to understand a trade-off, data migration, Shopify behavior, or follow-up task. Before committing, inspect both the staged content and the local diff.

```bash
git status
git diff
git add <file-or-directory>
git diff --staged
git commit -m "docs: add Road-Martt contributor guide"
```

## Pull Request Workflow

Publish the branch, then open a pull request against `main`. Pull requests create the shared review record for a proposed change and allow maintainers to discuss, approve, and merge it safely.[6]

```bash
git push -u origin docs/improve-storefront-setup
gh pr create --base main --fill
```

A strong pull request is easy to evaluate because it explains the decision, keeps the scope narrow, and includes proof that the change was checked.

| Pull request section | What to include |
|---|---|
| **Title** | The same concise, imperative intent as the primary commit, such as `docs: add Road-Martt contributor guide`. |
| **Summary** | A short paragraph describing what changed and why it matters to Road-Martt. |
| **Validation** | Exact commands run, their results, and any manual storefront checks performed. |
| **Risk and rollout** | Note Shopify data impact, configuration changes, feature flags, or rollback steps when applicable. |
| **Visual evidence** | Before-and-after screenshots for customer-facing layout or styling changes. |
| **Follow-up** | Explicitly list deliberately deferred work rather than hiding it in scope. |

Do not merge your own pull request until required checks and reviews are complete. Rebase or update the branch when `main` changes materially, resolve conflicts locally, repeat validation, and push the updated branch. Keep conversations constructive and resolve review comments with either the requested change or a clear technical rationale.

## Shopify and Security Boundaries

Road-Martt’s commerce backend should remain Shopify-backed. Do not duplicate checkout, payment, fulfillment, inventory, or customer-review systems in the repository. When working on Shopify integrations, request only the minimum necessary access, keep tokens server-side, and use environment variables or the platform’s managed secret settings. Never create fake customer reviews, ratings, or testimonials in code, fixtures, copy, or sample data.

## Keeping Documentation Current

Whenever a contribution adds a language runtime, package manager, framework, test suite, deployment process, environment variable, or Shopify integration, update `README.md` and this file in the same pull request. A new contributor should be able to start from a clean clone without relying on unpublished instructions.

## References

[1]: https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning "GitHub Docs: About secret scanning"
[2]: https://cli.github.com/manual/gh_auth_login "GitHub CLI Manual: gh auth login"
[3]: https://cli.github.com/manual/gh_auth_setup-git "GitHub CLI Manual: gh auth setup-git"
[4]: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens "GitHub Docs: Managing personal access tokens"
[5]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-permissions-and-visibility-of-forks "GitHub Docs: About permissions and visibility of forks"
[6]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests "GitHub Docs: About pull requests"
