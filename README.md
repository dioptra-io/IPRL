# IPRL — Retina Coordinator

This repository is the source of truth for version compatibility across the Retina network measurement system, which consists of three components: [retina-agent](https://github.com/dioptra-io/retina-agent), [retina-orchestrator](https://github.com/dioptra-io/retina-orchestrator), and [retina-generator](https://github.com/dioptra-io/retina-generator).

It contains no application code — only a compatibility matrix defining which component versions form a coherent Retina release.

## Components

| Repository | Description |
|---|---|
| [retina-agent](https://github.com/dioptra-io/retina-agent) | Measurement agent |
| [retina-orchestrator](https://github.com/dioptra-io/retina-orchestrator) | Orchestration and scheduling |
| [retina-generator](https://github.com/dioptra-io/retina-generator) | Probe list generator |

Shared API types are defined in [retina-commons](https://github.com/dioptra-io/retina-commons), a library dependency pinned in each component's `go.mod`.

## Compatibility Matrix

Each file in `versions/` defines a Retina release. The codename is an Alpine pass (see naming convention below), the content pins the compatible component versions.

Example `versions/col-de-sarenne.yaml`:
```yaml
name: Col de Sarenne
retina-agent:        v1.0.0
retina-orchestrator: v1.0.0
retina-generator:    v1.0.0
```

## Release Naming

Retina releases are named after Alpine passes. A codename represents a generation of the system — it changes on a major IPRL version bump (i.e. when a component introduces a breaking change). Patch and minor updates are reflected by updating the pinned versions within the current codename file.

## Cutting a Release

1. Verify the component versions have been tagged and their Docker images published.
2. If this is a major version bump, create `versions/<new-codename>.yaml` with the new Alpine pass name. Otherwise update the existing codename file with the new component versions.
3. Tag the coordinator repo `retina-X.Y.Z`.