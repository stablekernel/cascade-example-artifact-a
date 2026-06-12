# cascade-example-artifact-a

Source repository in the cascade cross-repo pipeline topology. Builds a shared artifact on every push to `src/`, then notifies the primary repository (`cascade-example-primary`) via the cascade External Update mechanism so the primary can pick up the new artifact and run its own promotion pipeline.

## Role

This repository acts as a satellite source: it owns the build step and publishes a reusable build workflow that the primary references. It does not run environment promotions itself.

## Reusable build workflow

**Path:** `.github/workflows/build-shared.yaml`

The primary (and any other consumer) can reference this workflow via:

```yaml
uses: stablekernel/cascade-example-artifact-a/.github/workflows/build-shared.yaml@main
```

**Inputs:**

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `environment` | string | no | Target environment (passed through for context) |
| `sha` | string | no | Git SHA to check out and build |
| `version` | string | no | Version tag to attach to the artifact |

**Outputs:**

| Output | Description |
|--------|-------------|
| `artifact_id` | Identifier of the produced artifact (format: `artifact-a-<sha>`) |

## Cascade config

- **Schema version:** 1
- **CLI version:** v0.1.0
- **Trunk branch:** main
- **Triggers:** `src/**`
- **Notify target:** `stablekernel/cascade-example-primary` (workflow: `external-update.yaml`)
