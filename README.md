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

- `artifactory_username`: Artifactory Username
- `artifactory_password`: Artifactory Password
- `gar_username`: GAR Username
- `gar_password`: GAR Password
- `gar_registry`: GAR Registry (default: "us-docker.pkg.dev/sysdig-artifact-registry-dev/gar-docker")
- `kind_needed`: Boolean if a Kubernetes In Docker cluster is needed for tests (default: false)
- `kind_version`: KinD version (default: "v0.19.0")
- `kind_config_path`: KinD config path (default: tests/kind-config.yaml)
- `kind_cluster_name`: KinD cluster name (default: kind)
- `kind_wait`: increase the timeout for KinD to check if the control plane is ready (default: 60s)
- `kind_kubectl_version`: the kubectl version to use with KinD (default: v1.24.15)
- `kind_log_level`: the KinD verbosity level (default: 0)
- `bats_version`: BATS version ("1.9.0")
- `envsubst_needed`: Indicate if envsubst is needed (default: "false")
- `taskfile_name`: Name of the taskfile task to be executed (default: "test")
- `quay_username`: quay Username
- `quay_password`: quay Password

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
      - uses: actions/checkout@v2
      - uses: draios/infra-action-test-runner@v1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          # this will trigger `task test` in the target repo
```
