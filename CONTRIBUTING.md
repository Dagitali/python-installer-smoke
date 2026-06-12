# Contributing Guidelines

Version-controllable contributions toward improving [this project][README] (its source code,
documentation, etc.) are welcome via GitHub's [pull request] process. By submitting a merge request,
you acknowledge and agree to licensing your contribution to [Dagitali LLC][owner].

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change.

- [Ways to Contribute](#ways-to-contribute)
- [Merge Request Process](#merge-request-process)
- [Protected-Branch GitFlow Workflow](#protected-branch-gitflow-workflow)
- [Maintainer Runbooks](#maintainer-runbooks)
- [Code of Conduct](#code-of-conduct)
- [Local Quality Gates](#local-quality-gates)

## Ways to Contribute

Not every valuable contribution needs to be a pull request. python-installer-smoke also benefits
from codeless contributions such as:

- Sponsorship through [GitHub Sponsors] or [Buy Me a Coffee][Buy Me a Coffee]
- Documentation corrections and clarity improvements
- Reproducible bug reports with environment details and sample inputs
- Validation of releases on different operating systems or Python versions
- Examples, tutorials, and workflow suggestions
- Issue triage and feedback on feature proposals
- Answering usage questions in GitHub Discussions

If you are looking for the lowest-friction way to help, strong documentation feedback and
reproducible bug reports are often as valuable as small code changes.

## Merge Request Process

1. Do not commit or push directly to the protected `main` or `develop` branches. Work from a topic
   branch and open a pull request instead.
2. Update the [README] with details of changes to the action interface. This includes new inputs,
   outputs, runner environment behavior, examples, or version-support expectations.
3. For release-affecting changes (CI, release automation, usage snippets, or stable public surface
   decisions), consult [.github/MAINTAINER-RUNBOOKS.md](.github/MAINTAINER-RUNBOOKS.md).
4. For the protected-branch protection settings, required CI checks, and the exact GitHub
   configuration needed to disable direct pushes, consult
   [.github/BRANCH-PROTECTION.md](.github/BRANCH-PROTECTION.md).
5. You may merge once the protected-branch protection requirements have been satisfied. The
   recommended baseline is one approval for `develop` and two approvals (one approval for small
   teams) for `main`. If you do not have permission to merge, request a maintainer review and
   handoff.
6. For release expectations and versioning rules, consult [RELEASE-POLICY.md](RELEASE-POLICY.md).
7. For the workflow split and trigger model, consult [CI-CD-WORKFLOWS.md](CI-CD-WORKFLOWS.md).

## Protected-Branch GitFlow Workflow

python-installer-smoke uses GitFlow-style branch roles, but protected branches change how those
branches should be finished.

- Treat `feature/*`, `release/*`, and `hotfix/*` as working branches.
- Treat `develop` and `main` as GitHub-managed integration branches.
- Do not rely on `git flow feature finish` or `git flow release finish` as the authoritative way to
  update protected branches. Those commands create local merges, but protected branches and required
  checks are enforced in GitHub.

Preferred flow:

1. Create a topic branch from the correct base branch.
2. Push the topic branch.
3. Open a pull request into the protected target branch. Feature branches should target `develop`;
   only `release/*` and `hotfix/*` should target `main`.
4. Let `.github/workflows/pr.yml` provide the required PR checks.
5. Merge the pull request in GitHub after the branch protection requirements pass.

Recommended branch mapping:

- `feature/*` -> pull request into `develop`
- `release/*` -> pull request into `main`
- `hotfix/*` -> pull request into `main`

After a release or hotfix lands on `main`, sync that change back into `develop` deliberately rather
than assuming a local GitFlow finish command already made both protected branches authoritative.

For small/solo maintainer teams, using pull requests for both feature and release branches is still
normal once protected branches are enabled. The repository owner can keep the PR workflow while
choosing lower-friction settings, such as fewer required approvals on `develop`, or an admin bypass
policy that is intentionally narrow and documented.

## Maintainer Runbooks

The repository keeps maintainer runbooks in
[.github/MAINTAINER-RUNBOOKS.md](.github/MAINTAINER-RUNBOOKS.md).

That file is intended for project-local operating procedures, such as:

- Feature branch integration under protected branches
- Release and hotfix branch finishing
- Syncing `main` back into `develop`
- Tagging and tag-triggered CD behavior

It lives under `.github/` because it is repository operations guidance rather than user-facing
action documentation.

Private operator procedures, credentials, incident-response steps, and recovery playbooks should
live outside the repository.

## Code of Conduct

All contributors are expected to honor and adhere to our [Code of Conduct] policy. As such, please
do the following:

1. Read it before making any contributions;
2. Follow it in all your interactions with the project.

## Local Quality Gates

python-installer-smoke maintains one supported contributor local quality workflow. Contributors
should use:

```bash
pre-commit run
```

For local Git hooks, python-installer-smoke uses a staged workflow:

```bash
pre-commit install --install-hooks
```

- `pre-commit` runs the fast hygiene and auto-fix hooks before a commit is created.
- `commit-msg` validates the commit message format.

[Buy Me a Coffee]: https://buymeacoffee.com/djrlj694
[Code of Conduct]: CODE_OF_CONDUCT.md
[GitHub Sponsors]: https://github.com/sponsors/Dagitali
[owner]: https://dagitali.com
[pull request]: https://github.com/Dagitali/python-installer-smoke/pulls
[README]: README.md
[SemVer]: http://semver.org
