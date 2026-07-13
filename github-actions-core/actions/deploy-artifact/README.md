# Deploy Artifact

This composite action handles artifact deployment to a Linux target path. It downloads a packaged ZIP archive from GitHub Packages, creates a rollback snapshot, extracts the payload, and applies standard file ownership and permissions.

## Inputs

- package-name (required): Logical name used to construct the package URL.
- version-tag (required): Version or tag to fetch from the package registry.
- deploy-target-path (required): Destination path for the deployment payload.
- github-token (required): Token with read access to the GitHub Packages resource.
- server-user (default: www-data): Linux user and group owner for deployed files.

## Example

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/github-actions-core/actions/deploy-artifact@v1
        with:
          package-name: my-application
          version-tag: v1.2.3
          deploy-target-path: /var/www/html
          github-token: ${{ secrets.GITHUB_TOKEN }}
          server-user: www-data
```

## Operational notes

- The action creates a rollback snapshot under /rollback using the package name and version tag.
- The deployment target path is cleaned before extraction so the new artifact replaces the prior contents.
- Ownership defaults to www-data:www-data and permissions are set to 0755 for directories and 0644 for files.
