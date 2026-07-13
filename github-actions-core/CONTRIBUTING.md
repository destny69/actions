# Contributing to github-actions-core

Thank you for contributing to this centralized GitHub Actions platform. This repository is intended to provide reusable modules that can be consumed across many applications and teams.

## Contribution workflow

1. Fork the repository or create a branch from the main branch in the internal organization mirror.
2. Create a new folder under actions/ for your new module.
3. Add an action.yml file that defines the composite action interface and execution steps.
4. Add a README.md in the same folder to describe the use case, inputs, outputs, and example invocation.
5. Update the repository documentation if the module changes the standard workflow contract.
6. Open a pull request with a concise summary of the change and the validation evidence.

## Module authoring standards

- Use the composite action pattern with clear input definitions.
- Keep shell scripts explicit with set -euo pipefail and meaningful comments.
- Make inputs self-documenting and default values safe.
- Avoid hard-coded organization-specific assumptions unless they are clearly parameterized.
- Prefer deterministic, auditable behavior for production use.

## Versioning guidance

- Tag releases using semantic versioning.
- Publish stable releases with tags such as v1, v1.2.0, or v1.2.3.
- Consumers should reference the module using the semantic tag form, for example:

```yaml
uses: your-org/github-actions-core/actions/my-new-module@v1
```

## Validation requirements

Before submitting a pull request, run the local validation checks:

```bash
docker run --rm -v "$PWD:/repo" -w /repo rhysd/actionlint:latest -color
python -m pip install --user yamllint
yamllint .github/workflows actions
```

If your module depends on additional packages or tools, document those requirements in the module README and the workflow examples.

## Review expectations

- Review for security, reusability, and operational safety.
- Ensure the action does not introduce ambiguous behavior or brittle environment assumptions.
- Confirm that the new module includes adequate documentation and a clear path for downstream adoption.
