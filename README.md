# Halo — GitHub Action

Scan your codebase for **children's privacy compliance risks** in your CI/CD pipeline. Results appear in GitHub's Security tab via SARIF integration.

> **COPPA 2.0 takes effect April 22, 2026.** Penalties up to $53,088 per violation per day.

## Quick Start

```yaml
name: Halo Compliance Scan
on: [push, pull_request]

jobs:
  halo:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write

    steps:
      - uses: actions/checkout@v4
      - uses: runhalo/action@v1
```

Halo scans your code, uploads results to GitHub's Security tab, and fails the check if critical violations are found.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `path` | `.` | Path to scan |
| `fail-on` | `critical` | Fail at this severity or above: `critical`, `high`, `medium`, `low`, `none` |
| `upload-sarif` | `true` | Upload results to GitHub Code Scanning |
| `severity-threshold` | `low` | Minimum severity to report |
| `framework` | (auto) | Override framework detection |

## Outputs

| Output | Description |
|--------|-------------|
| `violations` | Total violations found |
| `critical` | Critical severity count |
| `high` | High severity count |
| `score` | Compliance score (0-100) |
| `grade` | Compliance grade (A-F) |
| `scan-passed` | Whether the scan passed your threshold |

## Examples

### Fail on high severity or above

```yaml
- uses: runhalo/action@v1
  with:
    fail-on: high
```

### Scan a specific directory

```yaml
- uses: runhalo/action@v1
  with:
    path: ./src
```

### Scan only, never fail

```yaml
- uses: runhalo/action@v1
  with:
    fail-on: none
```

### Use outputs in subsequent steps

```yaml
- uses: runhalo/action@v1
  id: halo

- name: Check score
  if: steps.halo.outputs.score < 80
  run: echo "Score: ${{ steps.halo.outputs.score }}/100 (${{ steps.halo.outputs.grade }})"
```

## What It Scans For

25 COPPA rules included free. Covers:

- **Data collection** — PII in URLs, excessive data gathering, missing retention limits
- **Consent** — Missing parental consent, pre-checked checkboxes, weak age gates
- **Tracking** — Third-party ad trackers, analytics without child-directed flags
- **Security** — Unencrypted PII, weak defaults, XSS risks
- **Design** — Dark patterns, unmoderated chat, push notifications without consent

Unlock 180+ rules across 13 jurisdictions with a [Pro license](https://runhalo.dev/#pricing).

**Supported languages:** JavaScript, TypeScript, Python, Swift, Java, Kotlin, HTML, and more.

## Permissions

```yaml
permissions:
  contents: read           # Read repository contents
  security-events: write   # Upload SARIF to Code Scanning (optional)
```

## Links

- [Website](https://runhalo.dev)
- [GitHub](https://github.com/runhalo/halo)
- [CLI](https://www.npmjs.com/package/@runhalo/cli)
- [VS Code Extension](https://marketplace.visualstudio.com/items?itemName=runhalo.halo-vscode)

## License

Apache 2.0

---

*Halo is a developer tool. It does not guarantee compliance. Consult qualified legal counsel for your specific obligations.*
