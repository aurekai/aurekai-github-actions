<p align="center">
  <img src="https://raw.githubusercontent.com/aurekai/aurekai/main/assets/aurekai-logo.svg" alt="Aurekai" width="520" />
</p>

# `aurekai/aurekai-github-actions` · v0.8.0-alpha.5

Official GitHub Actions integration for Aurekai — composite action + 7 reusable workflow templates covering all core capability operators.

## Composite Action (`action.yml`)

Use any Aurekai operator in a single step:

```yaml
- uses: aurekai/aurekai-github-actions@main
  with:
    operator: doctor-deep        # operator name (see below)
    version: "0.8.0-alpha.5"    # Aurekai runtime version
    upload-artifacts: "true"    # upload .json result as GitHub artifact
```

### Operators

| `operator` | Description |
|---|---|
| `doctor-deep` | Runtime diagnostics — all binaries pass/fail |
| `manifest-verify` | Validate `artifact.json` — schema, bindings, proof refs |
| `model-memory-pack` | FPQ compress + pack model memory |
| `sae-audit` | Sparse Autoencoder feature audit |
| `semantic-cache-bench` | Cache hit rate / latency benchmark |
| `proof-bundle-export` | Export `.akproof` bundle for current run |
| `release-gate` | Registry presence + SLI + proof checks |

### Outputs

| Output | Description |
|---|---|
| `result` | Full JSON output from the operator |
| `proof-uri` | `aurekai://proof/...` URI emitted by the operator |
| `exit-code` | Exit code from `akai` |

### Full pipeline

```yaml
- uses: aurekai/aurekai-github-actions@main
  with:
    operator: doctor-deep
- uses: aurekai/aurekai-github-actions@main
  with:
    operator: manifest-verify
- uses: aurekai/aurekai-github-actions@main
  with:
    operator: proof-bundle-export
    proof-run-id: ${{ github.sha }}
- uses: aurekai/aurekai-github-actions@main
  with:
    operator: release-gate
    release-tag: ${{ github.ref_name }}
```

## Reusable Workflows

| Workflow | Trigger | Description |
|---|---|---|
| `aurekai-doctor-deep.yml` | `workflow_call` / `workflow_dispatch` | Deep runtime diagnostics |
| `aurekai-manifest-verify.yml` | `workflow_call` / `workflow_dispatch` | Manifest validation |
| `aurekai-model-memory-pack.yml` | `workflow_call` / `workflow_dispatch` | Model memory packaging |
| `aurekai-sae-audit.yml` | `workflow_call` / `workflow_dispatch` | SAE audit |
| `aurekai-semantic-cache-bench.yml` | `workflow_call` / `workflow_dispatch` | Cache benchmark |
| `aurekai-proof-bundle-export.yml` | `workflow_call` / `workflow_dispatch` | Proof bundle export |
| `aurekai-release-gate.yml` | `workflow_call` / `workflow_dispatch` | Release gate |
| `aurekai-full-pipeline.yml` | `push` to `main` / `workflow_dispatch` | Full 5-stage chained pipeline |

## Quick Start

Call the full pipeline from your own workflow:

```yaml
jobs:
  aurekai:
    uses: aurekai/aurekai-github-actions/.github/workflows/aurekai-full-pipeline.yml@main
    with:
      version: "0.8.0-alpha.5"
      release-tag: ${{ github.ref_name }}
```

