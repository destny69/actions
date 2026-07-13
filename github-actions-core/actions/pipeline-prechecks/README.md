# Pipeline Prechecks

This composite action provides a controlled pre-build gate for application repositories. It combines dependency auditing, static analysis, and secret scanning so that pipelines can fail early when high-risk issues appear.

## Inputs

- fail-on-audit-warnings (default: true): Causes the audit step to fail when findings meet the configured threshold.
- working-directory (default: .): Sets the directory in which package manifests are discovered.

## Examples

```yaml
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/github-actions-core/actions/pipeline-prechecks@v1
        with:
          fail-on-audit-warnings: true
          working-directory: .
```

## Notes

- The action will skip dependency auditing when no package manifest is present.
- Semgrep is configured to use the default ruleset.
- Gitleaks is used to scan the repository contents for committed secrets.
