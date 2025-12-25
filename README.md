# setup-probitas

Set up your GitHub Actions workflow with [Probitas](https://github.com/probitas-test/probitas), a scenario-based testing framework.

## Usage

### Basic Setup

The simplest setup installs the latest stable Probitas version:

```yaml
- uses: probitas-test/setup-probitas@v1
```

### Full Example Workflow

```yaml
name: Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: probitas-test/setup-probitas@v1

      - name: Run Probitas tests
        run: probitas run
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `probitas-version` | The Probitas version to install from GitHub releases. Can be a semver version or `latest`. | `latest` |

## Outputs

| Output | Description |
|--------|-------------|
| `probitas-version` | The Probitas version that was installed. |

## Examples

### Specify Version

```yaml
- uses: probitas-test/setup-probitas@v1
  with:
    probitas-version: "0.7.1"
```

### Matrix Testing

Test across multiple Probitas versions:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        probitas-version: ["latest", "0.7.0", "0.7.1"]
    steps:
      - uses: actions/checkout@v4

      - uses: probitas-test/setup-probitas@v1
        with:
          probitas-version: ${{ matrix.probitas-version }}

      - run: probitas run
```

### Run with Selectors and Tags

```yaml
- uses: probitas-test/setup-probitas@v1

- name: Run integration tests
  run: probitas run -s tag:integration

- name: Run with custom reporter
  run: probitas run --reporter json > results.json
```

### Multiple Operating Systems

```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: probitas-test/setup-probitas@v1
      - run: probitas run
```

## How It Works

This action performs the following steps:

1. **Install Probitas CLI**: Downloads and installs the pre-compiled Probitas CLI binary from GitHub releases
2. **Verify Installation**: Confirms Probitas is correctly installed and available in the PATH

## Versioning

This action follows semantic versioning. When a new version is released (e.g., `v1.0.0`), the following tags are automatically updated:

- `v1` - Points to the latest `v1.x.x` release
- `v1.0` - Points to the latest `v1.0.x` release

### Recommended Usage

```yaml
# Recommended: Use major version for automatic updates
- uses: probitas-test/setup-probitas@v1

# Alternative: Pin to major.minor for more control
- uses: probitas-test/setup-probitas@v1.0

# Alternative: Pin to exact version for maximum stability
- uses: probitas-test/setup-probitas@v1.0.0
```

Using `@v1` ensures you automatically receive bug fixes and new features within the v1 major version, while avoiding breaking changes.

## Related Projects

- [Probitas](https://github.com/probitas-test/probitas) - The main Probitas framework
- [Probitas CLI](https://github.com/probitas-test/cli) - The Probitas CLI

## License

See [LICENSE](LICENSE) file for details.
