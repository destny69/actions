# Zip and Upload Artifact

This composite action packages a build folder into a versioned ZIP archive and publishes that archive to GitHub Packages.

## Inputs

- build-folder (required): Path to the directory that should be archived.
- package-name (required): Logical package identifier used in the upload URL.
- github-token (required): Token with packages:write permissions.

## Example

```yaml
jobs:
  package:
    runs-on: ubuntu-latest
    permissions:
      packages: write
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/github-actions-core/actions/zip-and-upload@v1
        with:
          build-folder: dist
          package-name: my-application
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Packaging convention

The archive name follows the pattern:

```text
<package-name>-<tag>.zip
```

Where the tag is the Git tag name when present, otherwise a shortened Git SHA.
