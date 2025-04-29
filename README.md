# action-test-runner

This Action try to generalize the way in which you can start tests.
It aim to provide common dependencies for test environment like KinD, Bats and the like

## Requirement

This actions require all the tests in the target repo to be callable using `task test` as entry point, levering [taskfile](taskfile.dev)

The other minimal entrypoints required are

- `task test`
- `task test:clean`
- `task test:logs`

## Inputs

### Required

- `github_token`: GITHUB_TOKEN secret

## Optional

- `artifact_name`: Name of the artifact to be downloaded and imported (default: "image")
- `bats_version`: BATS version (default: "1.11.1")
- `envsubst_needed`: Indicate if envsubst is needed (default: "false")
- `gar_password`: GAR Password
- `gar_registry`: GAR Registry
- `gar_username`: GAR Username
- `image_original_tag`: Original tag of the image to be imported
- `import_image_artifact`: Boolean if the image artifact should be imported (default: false)
- `kind_cluster_name`: KinD cluster name (default: kind)
- `kind_config_path`: KinD config path (default: tests/kind-config.yaml)
- `kind_kubectl_version`: the kubectl version to use with KinD (default: v1.27.1) - this version cannot be updated until the utility cluster has been updated - DEVOPS-14991
- `kind_log_level`: the KinD verbosity level (default: 0)
- `kind_needed`: Boolean if a Kubernetes In Docker cluster is needed for tests (default: true)
- `kind_version`: KinD version (default: "v0.19.0") - this version cannot be updated until the utility cluster has been updated - DEVOPS-14991
- `kind_wait`: increase the timeout for KinD to check if the control plane is ready (default: 60s)
- `local_image_name`: Name of the image to be imported (default: "testimage:local")
- `quay_password`: quay Password
- `quay_username`: quay Username
- `taskfile_name`: Name of the taskfile task to be executed (default: "test")

## Example workflow

Perform all checks on pull requests

```yaml
name: tests

on:

  pull_request:
    types: [opened, review_requested, ready_for_review]

jobs:
  tests:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4.2.2
      - uses: draios/infra-action-test-runner@v1.3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # this will trigger `task test` in the target repo
```
