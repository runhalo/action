# Halo COPPA Scanner — GitHub Action

Scan your codebase for **COPPA 2.0 privacy risks** and **ethical design issues** in your CI/CD pipeline. Results appear in GitHub's Security tab via SARIF integration.

> **COPPA 2.0 takes effect April 22, 2026.** Penalties up to $53,088 per violation per day. Scan early.

## Quick Start

```yaml
name: COPPA Scan
on: [push, pull_request]

jobs:
  halo:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      security-events: write  # Required for SARIF upload

    steps:
      - uses: actions/checkout@v4
      - uses: runhalo/action@v1
```

That's it. Halo scans your code, uploads results to GitHub's Security tab, and fails the check if critical violations are found.

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `path` | `.` | Path to scan |
| `fail-on` | `critical` | Fail when violations at this severity or above are found: `critical`, `high`, `medium`, `low`, `none` |
| `upload-sarif` | `true` | Upload SARIF results to GitHub Code Scanning |
| `version` | `latest` | Halo CLI version to install |

## Outputs

| Output | Description |
|--------|-------------|
| `violations` | Total number of violations found |
| `critical` | Number of critical severity violations |
| `high` | Number of high severity violations |
| `score` | Compliance score (0-100) |
| `grade` | Compliance grade (A-F) |
| `sarif-file` | Path to SARIF results file |

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

### Never fail (scan only)

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
  run: echo "Compliance score is ${{ steps.halo.outputs.score }}/100 (${{ steps.halo.outputs.grade }})"
```

### Pin a specific version

```yaml
- uses: runhalo/action@v1
  with:
    version: '0.2.0'
```

## Permissions

The action requires the following permissions:

```yaml
permissions:
  contents: read           # Read repository contents
  security-events: write   # Upload SARIF to Code Scanning (optional)
```

If you disable SARIF upload (`upload-sarif: false`), only `contents: read` is needed.

## What It Scans

Halo scans for **20 COPPA 2.0 compliance rules** across 16+ languages:

- **Authentication & Age Verification** — Missing age gates, weak verification
- **Data Collection** — PII in URLs, excessive data gathering
- **Consent Management** — Missing parental consent, pre-checked checkboxes
- **Security** — HTTP endpoints, weak encryption, insecure defaults
- **Data Retention** — Missing deletion handlers, indefinite storage
- **Tracking & Analytics** — Third-party trackers, behavioral profiling
- **UI/Dark Patterns** — Manipulative design, hidden data collection

**Supported languages:** TypeScript, JavaScript, Python, Swift, Java, Kotlin, HTML, Vue, Svelte, PHP, C++, C#, SQL, and more.

## Scan History & Trends

The action caches scan history between runs. The step summary shows score trends:

> **Score:** 85/100 (B) ↑ from 73 last scan (+12)

## How It Works

1. Installs the [Halo CLI](https://www.npmjs.com/package/@runhalo/cli)
2. Scans your codebase for COPPA compliance patterns
3. Uploads results to GitHub's Security tab (SARIF)
4. Fails the check if violations exceed your severity threshold
5. Posts a summary table to the workflow run

## Not Legal Advice

Halo is a developer tool. It does not guarantee COPPA compliance. Always consult qualified legal counsel for your specific compliance obligations. See [runhalo.dev/terms.html](https://runhalo.dev/terms.html).

## Links

- [Halo Website](https://runhalo.dev)
- [Documentation](https://github.com/runhalo/halo)
- [npm: @runhalo/cli](https://www.npmjs.com/package/@runhalo/cli)
- [VS Code Extension](https://marketplace.visualstudio.com/items?itemName=runhalo.halo-vscode)

## License

Apache 2.0 — see [LICENSE](LICENSE)
