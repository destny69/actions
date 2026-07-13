# github-actions-core

github-actions-core is a centralized, reusable GitHub Actions library for enterprise CI/CD workflows. It provides standardized composite actions for pre-build verification, artifact packaging, and deployment so that multiple applications can inherit consistent security, packaging, and operational controls.

## What this repository contains

- actions/pipeline-prechecks: dependency auditing, SAST, and secret scanning before build execution.
- actions/zip-and-upload: build-folder validation, archive packaging, and GitHub Packages publication.
- actions/deploy-artifact: artifact retrieval, rollback preparation, extraction, and permission hardening.

## Repository layout

```text
github-actions-core/
├── .github/
│   └── workflows/
│       ├── test-and-validate.yml
│       └── pr-lint.yml
├── actions/
│   ├── pipeline-prechecks/
│   │   ├── action.yml
│   │   └── README.md
│   ├── zip-and-upload/
│   │   ├── action.yml
│   │   └── README.md
│   └── deploy-artifact/
│       ├── action.yml
│       └── README.md
├── CONTRIBUTING.md
└── README.md
```

## Design goals

- Standardize secure delivery pipelines across the organization.
- Keep composite actions reusable and versioned for downstream consumption.
- Encourage internal contribution through clear review and testing requirements.
- Support enterprise package publication through GitHub Packages.

## GitHub organization configuration

To use these actions with private repositories and internal consumers, configure the following in GitHub organization settings:

1. Enable GitHub Packages for the organization.
2. Navigate to Organization settings -> Packages -> Package access and grant appropriate access to the consuming repositories.
3. In each consuming repository, grant the workflow the minimum required permissions:
   - packages: write for publishing artifacts
   - contents: read for checkout operations
4. Ensure the repository visibility and package access rules match the intended trust boundary for private distribution.

## Example usage blueprint

The following workflow snippet shows how an application project can invoke the three modules in sequence:

```yaml
name: Enterprise Release
on:
  push:
    tags:
      - 'v*'

jobs:
  build-and-release:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v4

      - name: Pre-build validation
        uses: your-org/github-actions-core/actions/pipeline-prechecks@v1
        with:
          fail-on-audit-warnings: true
          working-directory: .

      - name: Build application
        run: |
          npm ci
          npm run build

      - name: Package and publish
        uses: your-org/github-actions-core/actions/zip-and-upload@v1
        with:
          build-folder: dist
          package-name: my-application
          github-token: ${{ secrets.GITHUB_TOKEN }}

      - name: Deploy artifact
        uses: your-org/github-actions-core/actions/deploy-artifact@v1
        with:
          package-name: my-application
          version-tag: ${{ github.ref_name }}
          deploy-target-path: /var/www/html
          github-token: ${{ secrets.GITHUB_TOKEN }}
          server-user: www-data
```

## Maintenance and governance

- Keep module interfaces stable and documented.
- Prefer backwards-compatible changes in minor releases.
- Validate all workflow and action syntax through the repository self-test workflow.
- Treat the repository as a shared platform resource and review changes through pull requests.
