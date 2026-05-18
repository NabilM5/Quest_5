# GitLab and GitHub Workflow Manual

## Purpose

This manual describes a practical GitLab/GitHub workflow for creating repositories, managing branches, tracking work with issues, reviewing changes with pull requests or merge requests, tagging releases, and maintaining project documentation.

The workflow assumes a small project team using `main` as the stable production branch and `develop` as the integration branch for completed work.

## Repository Creation

1. Create a new repository in GitHub or GitLab.
2. Choose a clear repository name that matches the project name.
3. Add a `README.md` during repository creation when possible.
4. Select a `.gitignore` template that matches the project language or framework.
5. Choose the correct visibility level:
   - Public for open source or coursework submissions intended to be visible.
   - Private for internal, personal, or sensitive work.
6. Clone the repository locally:

```bash
git clone https://github.com/<owner>/<repository>.git
cd <repository>
```

7. Confirm the remote is configured correctly:

```bash
git remote -v
```

### Recommended README Structure

A useful `README.md` should include:

- Project name
- Short description
- Setup instructions
- Usage examples
- Test instructions
- Contribution workflow
- License information, if applicable

### Recommended .gitignore Rules

The `.gitignore` file should exclude generated files, dependency folders, local environment files, build output, logs, and editor metadata. Examples include:

```gitignore
.env
*.log
node_modules/
__pycache__/
dist/
build/
.vscode/
.idea/
```

![Screenshot 1: Repository creation page](../docs/screenshots/01-repository-creation.png)

## Branch Management

A predictable branch strategy keeps stable code separate from active work.

### Main Branches

- `main`: stable branch used for releases and final reviewed code.
- `develop`: integration branch where approved feature branches are merged before release.

Create the `develop` branch from `main`:

```bash
git checkout main
git pull origin main
git checkout -b develop
git push -u origin develop
```

### Feature Branches

Create one branch per issue or task:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/issue-12-login-form
```

Use descriptive branch names:

- `feature/issue-12-login-form`
- `fix/issue-18-validation-error`
- `docs/issue-21-api-readme`
- `release/v1.0.0`

Avoid vague names such as `updates`, `new-code`, or `final-changes`.

### Keeping Branches Updated

Before opening a pull request or merge request, update the branch:

```bash
git checkout feature/issue-12-login-form
git fetch origin
git rebase origin/develop
```

If the team prefers merge commits instead of rebasing:

```bash
git merge origin/develop
```

![Screenshot 2: Branch list showing main, develop, and feature branch](../docs/screenshots/02-branch-management.png)

## Issue Tracking

Issues should describe work clearly enough that another contributor can understand the task without private context.

### Good Issue Template

```markdown
## Objective
Describe the desired outcome.

## Requirements
- [ ] Requirement one
- [ ] Requirement two
- [ ] Requirement three

## Acceptance Criteria
- [ ] The implementation does X
- [ ] Tests or verification steps are included
- [ ] Documentation is updated when needed
```

### Creating a Branch from an Issue

Use the issue number in the branch name:

```bash
git checkout develop
git pull origin develop
git checkout -b feature/issue-5-user-profile
```

Reference the issue in commits and pull requests:

```bash
git commit -m "Implement user profile page for issue #5"
```

In the pull request description, use closing keywords when the PR fully resolves the issue:

```markdown
Closes #5
```

Use non-closing references when the PR is partial:

```markdown
Related to #5
```

![Screenshot 3: Issue page with checklist and linked branch](../docs/screenshots/03-issue-tracking.png)

## Pull Request and Merge Request Workflow

Pull requests in GitHub and merge requests in GitLab provide a review boundary before code reaches shared branches.

### Before Opening a Request

Run local checks:

```bash
git status
git diff
git test
```

Use the project-specific test command when available, such as:

```bash
npm test
pytest
make test
```

Push the feature branch:

```bash
git push -u origin feature/issue-12-login-form
```

### Pull Request Description

A clear PR or MR should include:

```markdown
## Summary
- Describe the change.
- Mention important implementation details.

## Verification
- [ ] Ran tests locally
- [ ] Checked affected workflow manually

## Related Issue
Closes #12
```

### Review Checklist

Reviewers should check:

- The change matches the linked issue.
- The code is focused and avoids unrelated edits.
- Tests or manual verification are included.
- Documentation is updated when behavior changes.
- Naming and style match the project.
- No secrets, generated artifacts, or local files were committed.

### Merge Rules

Recommended rules for `main`:

- Require at least one approval.
- Require status checks to pass.
- Require branches to be up to date before merging.
- Disallow force pushes.
- Delete merged branches automatically.

Merge feature branches into `develop`. Merge `develop` into `main` only for release-ready code.

![Screenshot 4: Pull request review page with checks and approval](../docs/screenshots/04-pull-request-review.png)

## Version Tagging and Releases

Use semantic versioning when the project publishes releases:

```text
MAJOR.MINOR.PATCH
```

Examples:

- `v1.0.0`: first stable release
- `v1.1.0`: backward-compatible feature release
- `v1.1.1`: backward-compatible bug fix
- `v2.0.0`: breaking change release

### Creating a Tag

```bash
git checkout main
git pull origin main
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

### Release Notes

Release notes should include:

- Summary of the release
- New features
- Bug fixes
- Breaking changes
- Upgrade or migration notes
- Contributors, when appropriate

GitHub and GitLab both support creating release pages from tags.

![Screenshot 5: Release page showing tag and release notes](../docs/screenshots/05-release-management.png)

## Project Wiki and Documentation

Use a wiki or documentation folder for information that is larger than the README.

Recommended documentation pages:

- Project overview
- Installation guide
- Development workflow
- Architecture notes
- API reference
- Deployment guide
- Troubleshooting guide
- Contribution guide

For repository-based documentation, use this structure:

```text
docs/
  screenshots/
  architecture.md
  development.md
  deployment.md
  troubleshooting.md
```

For wiki documentation, keep page names clear and link related pages from a home page.

## End-to-End Workflow Example

1. Create issue `#12` for a new feature.
2. Create branch `feature/issue-12-login-form` from `develop`.
3. Make focused commits on the feature branch.
4. Push the branch to GitHub or GitLab.
5. Open a PR or MR into `develop`.
6. Link the issue in the request description.
7. Run CI checks and fix failures.
8. Request review from a teammate.
9. Address review comments.
10. Merge after approval and passing checks.
11. Delete the feature branch.
12. When ready, merge `develop` into `main`.
13. Tag the release and publish release notes.
14. Update wiki or documentation pages.

## Best Practices

- Commit small, focused changes.
- Write commit messages in the imperative mood, such as `Add login validation`.
- Pull before starting new work.
- Keep feature branches short-lived.
- Open draft PRs early for visibility when work is not finished.
- Never commit secrets, passwords, tokens, or private keys.
- Protect `main` and require review before merging.
- Use issues to track planned work and bugs.
- Keep documentation updated with workflow changes.
- Close issues through merged PRs only when all acceptance criteria are complete.

## Screenshot Checklist

The issue requires at least five supporting screenshots. Add these files before marking the issue complete:

- `docs/screenshots/01-repository-creation.png`
- `docs/screenshots/02-branch-management.png`
- `docs/screenshots/03-issue-tracking.png`
- `docs/screenshots/04-pull-request-review.png`
- `docs/screenshots/05-release-management.png`

Each screenshot should show the actual GitHub or GitLab page described by the matching section.
