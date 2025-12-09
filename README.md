# setup-probitas

Set up your GitHub Actions workflow with [Probitas](https://github.com/jsr-probitas/probitas), a scenario-based testing framework for Deno.

## Usage

### Basic Setup

The simplest setup installs the latest stable Deno and Probitas versions:

```yaml
- uses: jsr-probitas/setup-probitas@v1
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

      - uses: jsr-probitas/setup-probitas@v1
        with:
          deno-version: 2.x
          probitas-version: latest
          cache: true

      - name: Run Probitas tests
        run: probitas run
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `deno-version` | The Deno version to install. Can be a semver version (e.g., `2.0.0`, `2.x`), `canary`, `lts`, or a Git hash. | `2.x` |
| `probitas-version` | The Probitas version to install from JSR. Can be a semver version or `latest`. | `latest` |
| `cache` | Cache downloaded modules & packages automatically in GitHub Actions cache. | `true` |
| `cache-hash` | A hash used as part of the cache key, which defaults to a hash of the deno.lock files. | - |

## Outputs

| Output | Description |
|--------|-------------|
| `cache-hit` | A boolean indicating whether the cache was hit. |
| `deno-version` | The Deno version that was installed. |
| `probitas-version` | The Probitas version that was installed. |

## Examples

### Specify Versions

```yaml
- uses: jsr-probitas/setup-probitas@v1
  with:
    deno-version: "2.1.0"
    probitas-version: "0.1.0"
```

### Use LTS Deno

```yaml
- uses: jsr-probitas/setup-probitas@v1
  with:
    deno-version: lts
```

### Disable Cache

```yaml
- uses: jsr-probitas/setup-probitas@v1
  with:
    cache: false
```

### Custom Cache Key

```yaml
- uses: jsr-probitas/setup-probitas@v1
  with:
    cache-hash: ${{ hashFiles('**/deno.json', '**/deno.lock') }}
```

### Matrix Testing

Test across multiple Deno versions:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        deno-version: ["2.x", lts, "2.0.0"]
    steps:
      - uses: actions/checkout@v4

      - uses: jsr-probitas/setup-probitas@v1
        with:
          deno-version: ${{ matrix.deno-version }}

      - run: probitas run
```

### Run with Selectors and Tags

```yaml
- uses: jsr-probitas/setup-probitas@v1

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
      - uses: jsr-probitas/setup-probitas@v1
      - run: probitas run
```

## How It Works

This action performs the following steps:

1. **Setup Deno**: Uses [denoland/setup-deno](https://github.com/denoland/setup-deno) to install Deno with the specified version and cache configuration
2. **Install Probitas CLI**: Installs the Probitas CLI from JSR using `deno install`
3. **Verify Installation**: Confirms both Deno and Probitas are correctly installed and available in the PATH

## Caching

Caching is enabled by default and helps speed up your workflows by:

- Caching Deno's compiled module cache
- Caching downloaded dependencies based on your `deno.lock` file
- Reducing network requests and installation time

The cache key is automatically generated based on:
- The GitHub job ID
- The runner OS and architecture
- A hash of `deno.lock` files in your project (customizable via `cache-hash`)

## Related Projects

- [Probitas](https://github.com/jsr-probitas/probitas) - The main Probitas framework
- [setup-deno](https://github.com/denoland/setup-deno) - Deno setup action (used internally)

## License

See [LICENSE](LICENSE) file for details.
