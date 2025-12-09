# Basic Probitas Example

This is a minimal example project demonstrating Probitas usage with setup-probitas GitHub Action.

## Local Usage

```bash
# Install Probitas CLI
deno install -grAf -n probitas jsr:@probitas/cli

# Run scenarios
probitas run

# Run with specific tags
probitas run -s tag:example
```

## GitHub Actions Usage

```yaml
- uses: jsr-probitas/setup-probitas@v1

- name: Run tests
  run: probitas run
```

## Project Structure

```
.
├── deno.json                    # Deno configuration with Probitas settings
└── probitas/
    └── example.probitas.ts      # Example test scenario
```

## Learn More

- [Probitas Documentation](https://github.com/jsr-probitas/probitas)
- [setup-probitas README](../../README.md)
