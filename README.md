# Aikido Safe Chain Action

A GitHub Action that installs and configures [Aikido Safe Chain](https://github.com/AikidoSec/safe-chain) to protect your CI/CD pipeline from malicious packages.

Safe Chain intercepts package installations and blocks known malware in real-time for **npm, yarn, pnpm, bun, pip, uv, poetry** and more.

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: luisfontes19/aikido-safe-chain-action@v1

  - run: npm ci   # automatically protected by safe-chain
```

### With all options

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: luisfontes19/aikido-safe-chain-action@v1
    with:
      logging: silent
      minimum-package-age-hours: "48"
      npm-minimum-package-age-exclusions: "@myorg/*,trusted-pkg"
      npm-custom-registries: "npm.company.com,registry.internal.net"
      pip-custom-registries: "pip.company.com"

  - run: npm ci
```

### Pin a specific version

```yaml
- uses: luisfontes19/aikido-safe-chain-action@v1
  with:
    version: "1.3.2"
```

## Inputs

| Input | Description | Required | Default |
|---|---|---|---|
| `version` | Pin a specific safe-chain version (e.g. `1.3.2`). Leave empty for latest. | No | latest |
| `logging` | Logging level: `silent`, `normal`, or `verbose`. | No | `normal` |
| `minimum-package-age-hours` | Minimum age (hours) a package must exist before install is allowed (npm only). | No | `24` |
| `npm-minimum-package-age-exclusions` | Comma-separated packages/scopes excluded from age filtering (e.g. `@aikidosec/*`). | No | |
| `npm-custom-registries` | Comma-separated custom npm registries to scan. | No | |
| `pip-custom-registries` | Comma-separated custom pip/PyPI registries to scan. | No | |

## How it works

1. **Installs** Aikido Safe Chain via the official installer with the `--ci` flag (uses PATH shims instead of shell aliases).
2. **Configures** safe-chain parameters as environment variables (`SAFE_CHAIN_*`) available to all subsequent workflow steps.
3. **Adds** safe-chain shims to `$GITHUB_PATH` so every subsequent `npm`, `pip`, `yarn`, etc. call is automatically protected.

## Environment variables set

When inputs are provided, the action exports these variables for all subsequent steps:

| Input | Environment Variable |
|---|---|
| `logging` | `SAFE_CHAIN_LOGGING` |
| `minimum-package-age-hours` | `SAFE_CHAIN_MINIMUM_PACKAGE_AGE_HOURS` |
| `npm-minimum-package-age-exclusions` | `SAFE_CHAIN_NPM_MINIMUM_PACKAGE_AGE_EXCLUSIONS` |
| `npm-custom-registries` | `SAFE_CHAIN_NPM_CUSTOM_REGISTRIES` |
| `pip-custom-registries` | `SAFE_CHAIN_PIP_CUSTOM_REGISTRIES` |

## Supported package managers

All package managers supported by Aikido Safe Chain:

npm · npx · yarn · pnpm · pnpx · bun · bunx · pip · pip3 · uv · poetry · pipx

## License

MIT
