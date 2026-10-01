# Docker image build (ECR and/or GCP Artifact Registry)

Reusable workflow: `.github/workflows/build-docker-image.yaml`

Builds a Docker image once and pushes it to **Amazon ECR**, **GCP Artifact Registry** (often called “GCR” in ReportPortal repos), or **both**.

## `registry` input

| Value | Destination |
| --- | --- |
| `ecr` (default) | Amazon ECR only — existing callers keep working |
| `gcr` | GCP Artifact Registry only |
| `both` | Same build pushed to ECR and Artifact Registry |

## ECR usage (unchanged)

```yaml
jobs:
  call-docker-build:
    uses: reportportal/.github/.github/workflows/build-docker-image.yaml@main
    with:
      aws-region: ${{ vars.AWS_REGION }}
      image-tag: ${{ needs.variables-setup.outputs.tag }}
      additional-tag: 'develop-latest'
    secrets:
      AWS_ROLE_ARN: ${{ secrets.AWS_ROLE_ARN }}
```

Required secret: `AWS_ROLE_ARN`.

## GCR / Artifact Registry usage

```yaml
jobs:
  call-docker-build:
    uses: reportportal/.github/.github/workflows/build-docker-image.yaml@main
    with:
      registry: gcr
      gcr-region: ${{ vars.GCR_DEV_REGION }}   # e.g. europe-docker.pkg.dev
      gcp-project: ${{ vars.GCP_DEV_PROJECT }}
      image-tag: ${{ needs.variables-setup.outputs.tag }}
      additional-tag: 'develop-latest'
    secrets:
      GCP_WORKLOAD_IDENTITY_PROVIDER: ${{ secrets.GCP_DEV_WORKLOAD_IDENTITY_PROVIDER }}
      GCP_SERVICE_ACCOUNT: ${{ secrets.GCP_DEV_SERVICE_ACCOUNT }}
      GCR_REGION: ${{ secrets.GCR_DEV_REGION }}       # optional if gcr-region input is set
      GCP_PROJECT: ${{ secrets.GCP_DEV_PROJECT }}     # optional if gcp-project input is set
```

Image path default: `${GCR_REGION}/${GCP_PROJECT}/reportportal/<repo-name>:<tag>`  
Override with `gcr-repository` (e.g. `reportportal/service-marketplace`).

Required secrets when `registry` is `gcr` or `both`:
- `GCP_WORKLOAD_IDENTITY_PROVIDER`
- `GCP_SERVICE_ACCOUNT`

Plus project/region via secrets **or** inputs:
- `GCR_REGION` / `gcr-region` (hostname such as `europe-docker.pkg.dev`)
- `GCP_PROJECT` / `gcp-project`

## Both registries

```yaml
with:
  registry: both
  aws-region: ${{ vars.AWS_REGION }}
  gcr-region: ${{ vars.GCR_REGION }}
  gcp-project: ${{ vars.GCP_PROJECT }}
secrets:
  AWS_ROLE_ARN: ${{ secrets.AWS_ROLE_ARN }}
  GCP_WORKLOAD_IDENTITY_PROVIDER: ${{ secrets.GCP_WORKLOAD_IDENTITY_PROVIDER }}
  GCP_SERVICE_ACCOUNT: ${{ secrets.GCP_SERVICE_ACCOUNT }}
```

## Related workflows

- `promote-ecr-to-gcr.yaml` — copy an **existing** ECR image into Artifact Registry (release promotion). Prefer that when ECR is already the source of truth.
- `aws-oidc-auth.yaml` — standalone AWS OIDC assume-role helper (see `oidc-auth-usage.md`).
