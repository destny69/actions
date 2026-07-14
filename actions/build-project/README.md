# Build Project

This composite action prepares a build output directory after prechecks and can optionally run a project build command. It is intended as a bridge between the precheck gate and downstream packaging steps.

## Inputs

- working-directory (default: .): Directory containing the project source and package manifest.
- build-folder (default: build): Path to the directory that should be created for build output.
- build-command (optional): A custom shell command to run when a project-specific build command is needed.
- package-manager (optional): Override for automatic build detection when the repository uses a specific package manager.

## Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/github-actions-core/actions/pipeline-prechecks@v1
        with:
          fail-on-audit-warnings: true
          working-directory: .
      - uses: your-org/github-actions-core/actions/build-project@v1
        with:
          working-directory: .
          build-folder: dist
          build-command: npm run build
```

## Notes

- If no build command is supplied and a package manifest is present, the action will attempt to run a standard build script using the detected package manager.
- If the project does not use a package manifest, the action simply creates the requested build folder.
