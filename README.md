# IPRL — Retina Coordinator

This repository is the source of truth for version compatibility across the Retina network measurement system, which consists of three roles: a PD source, the orchestrator and the agent. The PD source is either [retina-generator](https://github.com/dioptra-io/retina-generator) or [retina-iris-pds](https://github.com/dioptra-io/retina-iris-pds).

It contains no application code — only a compatibility matrix defining which component versions form a coherent Retina release.

## Components

| Repository | Role | Description |
|---|---|---|
| [retina-agent](https://github.com/dioptra-io/retina-agent) | Agent | Measurement agent |
| [retina-orchestrator](https://github.com/dioptra-io/retina-orchestrator) | Orchestrator | Orchestration and scheduling |
| [retina-generator](https://github.com/dioptra-io/retina-generator) | PD source | Random Probing Directive generator |
| [retina-iris-pds](https://github.com/dioptra-io/retina-iris-pds) | PD source | Iris-derived Probing Directives |

The two PD sources are interchangeable: both produce PDs as JSONL for the orchestrator. Each release pins the source(s) it uses. If a release uses both, both are listed.

Shared API types are defined in [retina-commons](https://github.com/dioptra-io/retina-commons), a library dependency pinned in each Go component's `go.mod`.

## Compatibility Matrix

Each file in `versions/` defines a Retina release. The codename is an Alpine pass (see naming convention below), the content pins the compatible component versions.

Example `versions/col-de-sarenne.yaml` (random PDs):
```yaml
name: Col de Sarenne
retina-agent:        v1.0.1
retina-orchestrator: v1.0.1
retina-generator:    v1.0.1
```

Example `versions/col-du-lautaret.yaml` (Iris-derived PDs):
```yaml
name: Col du Lautaret
retina-agent:        v2.0.0
retina-orchestrator: v2.0.0
retina-iris-pds:     v0.1.0
```

## Release Naming

Retina releases are named after Alpine passes. A codename represents a generation of the system — it changes on a major IPRL version bump (i.e. when a component introduces a breaking change). Patch and minor updates are reflected by updating the pinned versions within the current codename file.

Each codename tag marks the start of that generation. To find the pins of a specific release, use the `retina-X.Y.Z` tag.

## Cutting a Release

1. Verify the component versions have been tagged and, where applicable, their Docker images published.
2. If this is a major version bump, create `versions/<new-codename>.yaml` with the new Alpine pass name. Otherwise update the existing codename file with the new component versions.
3. Tag the coordinator repo `retina-X.Y.Z`. For a new codename, also create a `<codename>` tag (e.g. `col-du-lautaret`) on the commit that adds its `versions/` file. This tag does not move on later patch releases.