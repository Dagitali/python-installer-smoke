# Release Policy And Versioning

This document describes the public release policy for python-installer-smoke, including versioning
expectations, release channels, deprecation posture, and release-note handling.

- [Scope](#scope)
- [Supported Release Line](#supported-release-line)
- [Versioning Model](#versioning-model)
- [Patch Releases](#patch-releases)
- [Minor Releases](#minor-releases)
- [Major Releases](#major-releases)
- [Deprecation Policy](#deprecation-policy)
- [Release Artifacts](#release-artifacts)
- [Release Notes](#release-notes)

## Scope

This file is a public policy document. It explains what maintainers and contributors should expect
from python-installer-smoke releases, but it does not contain private operator steps, credentials,
or incident response procedures.

## Supported Release Line

python-installer-smoke currently treats the `v1.x` line as the stable public release line.

The supported surface is the documented GitHub Actions action interface, including action inputs,
outputs, side effects on the runner environment, and the implemented runtime capabilities explicitly
described in the public documentation. Historical files, placeholder paths, and stubbed
implementations are not part of the stable contract unless they are promoted in the public docs.

## Versioning Model

python-installer-smoke follows semantic-versioning-style expectations for public releases:

- `MAJOR` releases are for intentionally breaking public changes
- `MINOR` releases add backward-compatible public capability
- `PATCH` releases are for backward-compatible fixes and release-hygiene corrections

Version tags use the format `v*.*.*` and are expected to be annotated tags.

## Patch Releases

Patch releases are appropriate for changes such as:

- Bug fixes that preserve the documented public contract
- CI/CD or release-automation fixes that do not change the supported interface
- Documentation corrections that align public docs with actual stable behavior
- Dependency or compatibility fixes that preserve supported usage

Patch releases should avoid widening or narrowing the stable surface unless the change is purely a
documentation correction for behavior that was already intentionally supported.

## Minor Releases

Minor releases are appropriate for changes such as:

- New documented GitHub Actions action inputs or outputs
- Additional supported runner operating systems or Python versions
- Additive configuration capabilities that remain backward compatible

Minor releases may expand the public surface, but they should not silently break existing documented
CI/CD workflows.

## Major Releases

Major releases are appropriate when python-installer-smoke intentionally breaks the documented
public contract.

Examples include:

- Removing a previously documented stable GitHub Actions action input or output
- Changing expected input, output, or runner-environment behavior incompatibly
- Removing support for a previously supported runtime or platform without a compatible fallback

Major releases should be accompanied by explicit migration guidance.

## Deprecation Policy

python-installer-smoke prefers a documented deprecation period before removing a stable public
capability.

When practical:

- Mark the capability as deprecated in user-facing docs
- Provide a replacement path when one exists
- Preserve the deprecated behavior through at least one stable minor release before removal
- Preserve the deprecated behavior through at least one stable minor release before removal

Immediate removal without a deprecation window should be reserved for exceptional cases such as
security, legal, or clearly broken accidental surfaces that were never intended to be stable.

## Release Artifacts

Public releases are expected to produce:

- A Git tag for the released version
- A GitHub Release for announcement and release-note publication
- The tagged repository source used by downstream CI/CD workflows with `uses:`

Tagged release automation is handled by `.github/workflows/cd.yml`.

## Release Notes

GitHub Releases are the canonical public release-history surface for python-installer-smoke.

Release notes should summarize:

- User-visible features or fixes
- Deprecations
- Breaking changes
- Notable compatibility or CI/CD workflow changes when they affect users or contributors

For release-readiness execution details, consult `.github/MAINTAINER-RUNBOOKS.md`. For public CI/CD
workflow roles and triggers, consult `CI-CD-WORKFLOWS.md`.
