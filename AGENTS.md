# AGENTS.md — AI Agent Instructions for migtools/kubevirt-velero-plugin

## Project Overview
KubeVirt Velero Plugin enables [Velero](https://velero.io/) to correctly backup and restore KubeVirt virtual machines in Kubernetes/OpenShift clusters. The plugin handles the complexities of VM state (running/stopped), associated resources (DataVolumes, PVCs, VMIs), and ensures consistent backups of virtual machine workloads in the OADP ecosystem.

- **Primary Language**: Go
- **Module**: `kubevirt.io/kubevirt-velero-plugin`
- **Default Branch**: `main`

## Build Instructions
```bash
# Build the plugin binary
make build-local

# Build all binaries
make build-all

# Build container image
make build-image

# Build and push container
make push
```

## Test Instructions
```bash
# Run unit tests
make test

# Run functional/e2e tests (requires cluster)
make test-functional

# Run functional tests locally
make test-functional-local

# Run specific tests
go test ./pkg/... -run TestName

# Vet code
go vet ./...
```

## Code Conventions
- Plugin implementations in `pkg/`
- CLI and plugin registration in `cmd/`
- Functional tests in `tests/`
- Uses vendored dependencies (`vendor/` directory)
- Follow Velero plugin interface patterns
- KubeVirt API types used extensively

## Project Structure
```
cmd/           - Plugin binary entry point and registration
pkg/           - Plugin implementation
  plugin/      - Velero plugin implementations (BIA, RIA)
  util/        - Utility functions for KubeVirt resources
tests/         - Functional/e2e tests
vendor/        - Vendored Go dependencies
cluster-up/    - Scripts for local cluster setup
example/       - Example Velero backup/restore manifests
hack/          - Build and CI scripts
```

## CI/CD
- GitHub Actions workflows in `.github/workflows/`:
  - `coverall.yml` — Code coverage reporting
- Reproduce CI locally:
  ```bash
  make build-local
  make test
  ```

## Dependency Management
```bash
# Update Go modules
make gomod-update

# This runs both:
make modules  # go mod tidy
make vendor   # go mod vendor
```

## Common Tasks

### Adding a new Velero plugin action
1. Create a new plugin file in `pkg/plugin/`
2. Implement the Velero `BackupItemAction` or `RestoreItemAction` interface
3. Register the plugin in `cmd/`
4. Add unit tests in `pkg/plugin/`
5. Add functional tests in `tests/`

### Handling a new KubeVirt resource type
1. Add resource handling in `pkg/plugin/`
2. Implement backup/restore logic for the new resource
3. Add utility functions in `pkg/util/` if needed
4. Test with example manifests in `example/`
