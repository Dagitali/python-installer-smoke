# CI/CD Workflow Map

This document explains the public role of each GitHub Actions workflow used by
python-installer-smoke.

- [Scope](#scope)
- [Workflow Overview](#workflow-overview)
- [PR Gates](#pr-gates)
- [CI](#ci)
- [Release](#release)
- [How They Interact](#how-they-interact)
- [Required Checks](#required-checks)

## Scope

This file is public and describes the visible workflow structure. It does not include secrets,
credential handling, or emergency operator procedures.

## Workflow Overview

python-installer-smoke currently separates automation into three workflows:

- `pr.yml` for required pull-request and merge-queue gates
- `ci.yml` for cross-platform action compatibility validation
- `cd.yml` for tagged release publication

This split keeps required PR checks fast enough to use as branch-protection gates while moving the
larger operating-system and Python-version matrix into CI.

## PR Gates

Workflow file: `.github/workflows/pr.yml`

Workflow name: `PR Gates`

Primary role:

- Run the required validation used by protected-branch pull requests and merge queues

Current responsibilities:

- GitFlow pull-request target validation
- Root action metadata presence check
- YAML syntax validation for GitHub configuration files
- Fast local-action smoke test on Ubuntu with the default smoke-test Python version
- Optional input smoke test for `pip-version`, `upgrade-pip`, and `pip-install`

This workflow runs on pull requests into protected branches and also on pushes to the working and
release-oriented branches that feed later validation.

## CI

Workflow file: `.github/workflows/ci.yml`

Workflow name: `CI`

Primary role:

- Run cross-platform compatibility validation for the repository action

Current responsibilities:

- Execute the checked-out action with a matrix of GitHub-hosted runner operating systems
- Verify that the requested Python versions become active after the action runs
- Smoke-test optional pip and dependency installation inputs on each supported operating system

This workflow runs on pull requests into `main` and `develop`, on `merge_group` for those same
protected branches, and manually via `workflow_dispatch`.

## Release

Workflow file: `.github/workflows/cd.yml`

Workflow name: `Release`

Primary role:

- Validate and publish tagged GitHub Action releases

Current responsibilities:

- Run the tagged action through the same cross-platform compatibility matrix used by CI
- Validate optional pip and dependency installation inputs before release publication
- Publish a GitHub Release with generated release notes

This workflow is tag-driven. It runs when a `v*.*.*` tag is pushed.

## How They Interact

The workflows intentionally do not form a single linear chain.

- `PR Gates` is the required branch-protection workflow.
- `CI` runs alongside `PR Gates` when protected-branch pull requests or merge-queue entries need the
  heavier action compatibility matrix.
- `Release` does not run because `CI` succeeded; it runs only when a release tag is pushed.

That means a successful `ci.yml` run is a confidence signal, not a publication trigger by itself.

## Required Checks

Protected branches must require checks from `PR Gates`. They may also require checks from `CI` when
you want the full operating-system and Python-version matrix to block merges. The exact
protected-branch settings and GitHub configuration details are maintained separately in
`.github/BRANCH-PROTECTION.md`.
