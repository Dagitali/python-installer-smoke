# python-installer-smoke

Smoke-test a built Python wheel across supported command-line installers in GitHub Actions.

`python-installer-smoke` is a composite GitHub Action for package release pipelines. It installs a
wheel artifact with `pip`, `pipx`, and/or `uv`, then runs caller-provided smoke commands with the
installed command on `PATH`.

Use this after building and validating a wheel, before publishing or tagging a release. The action
is not a replacement for unit tests or package-build validation; it verifies that the built artifact
can be installed through the installer paths your users are likely to use.

- [Usage](#usage)
- [Inputs](#inputs)
- [Examples](#examples)
- [Behavior](#behavior)
- [Security Notes](#security-notes)
- [Outputs](#outputs)
- [Permissions](#permissions)
- [Version Pinning](#version-pinning)
- [License](#license)

## Usage

Build a wheel, then smoke-test it with all supported installers:

```yaml
jobs:
  installer-smoke:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6

      - name: Build wheel
        run: |
          python -m pip install --upgrade build
          python -m build --wheel

      - name: Smoke-test supported installers
        uses: Dagitali/python-installer-smoke@v1
        with:
          artifact-wheel: dist/*.whl
          command-name: my-command
          package-name: my-package
          smoke-commands: |
            my-command --version
            my-command --help
```

Use the action from a checked-out copy when developing this repository:

```yaml
steps:
  - uses: actions/checkout@v6
  - uses: ./
    with:
      artifact-wheel: dist/*.whl
      command-name: my-command
      smoke-commands: |
        my-command --version
```

## Inputs

| Name | Required | Default | Description |
| --- | --- | --- | --- |
| `artifact-wheel` | No | `dist/*.whl` | Path or glob for the built wheel artifact to install. |
| `command-name` | Yes | N/A | Command expected to be available on `PATH` after installing the wheel. |
| `installer-smoke-installers` | No | `pip,pipx,uv` | Comma-separated installers to test. Supported values are `pip`, `pipx`, and `uv`. |
| `package-name` | No | `''` | Distribution package name used for installer cleanup. Defaults to `command-name` when unset. |
| `smoke-commands` | Yes | N/A | Newline-separated commands to run after each installer path. |
| `venv-path` | No | `.installer-pip-venv` | Temporary virtual environment path used for the `pip` install check. |

## Examples

Smoke-test only `pip`:

```yaml
- uses: Dagitali/python-installer-smoke@v1
  with:
    artifact-wheel: dist/*.whl
    command-name: my-command
    installer-smoke-installers: pip
    smoke-commands: |
      my-command --version
```

Smoke-test `pipx` and `uv`, but skip `pip`:

```yaml
- uses: Dagitali/python-installer-smoke@v1
  with:
    artifact-wheel: dist/*.whl
    command-name: my-command
    package-name: my-package
    installer-smoke-installers: pipx,uv
    smoke-commands: |
      my-command --version
      my-command check
```

Use a custom pip smoke-test virtual environment path:

```yaml
- uses: Dagitali/python-installer-smoke@v1
  with:
    artifact-wheel: dist/*.whl
    command-name: my-command
    installer-smoke-installers: pip
    venv-path: .tmp/installer-smoke-pip
    smoke-commands: |
      my-command --help
```

## Behavior

- `pip` installs the wheel into a temporary virtual environment and runs smoke commands with that
  environment's script directory first on `PATH`.
- `pipx` installs `pipx` into the runner Python environment, installs the wheel as a pipx app, runs
  smoke commands with `PIPX_BIN_DIR` first on `PATH`, then uninstalls the package.
- `uv` installs `uv` into the runner Python environment, installs the wheel as a uv tool, and runs
  smoke commands with `UV_TOOL_BIN_DIR` first on `PATH`.

The action expects `python` to be available on `PATH`. GitHub-hosted runners provide this by
default. If your workflow selects a specific Python version, run `actions/setup-python` before this
action.

## Security Notes

Treat `smoke-commands`, `artifact-wheel`, and `installer-smoke-installers` as trusted workflow
configuration. Do not build these values from untrusted issue, pull request, or user-supplied text.

The smoke commands are shell commands. Keep them small and deterministic, such as `--version`,
`--help`, or a lightweight command that validates startup and import behavior.

## Outputs

This action does not define outputs.

## Permissions

The action itself does not require repository or token permissions. Calling workflows should grant
only the permissions needed by their own jobs.

## Version Pinning

Prefer a release tag such as `Dagitali/python-installer-smoke@v1` or an exact version tag for normal
use. Pin to a full commit SHA when your workflow requires maximum supply-chain immutability.

## License

Copyright © 2026 Dagitali LLC. All rights reserved.

See [LICENSE](LICENSE) for details.
