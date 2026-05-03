# Quickstart — aurekai-github-actions

Reusable GitHub Actions workflows for the Aurekai pipeline.

## Use a Workflow

```yaml
jobs:
  doctor:
    uses: aurekai/aurekai-github-actions/.github/workflows/aurekai-doctor-deep.yml@main
```

## Validate Locally

```bash
bash tests/validate-schemas.sh
bash tests/validate-scripts.sh
```
