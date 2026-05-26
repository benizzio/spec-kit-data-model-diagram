# Data Model Diagram Extension

Generate a raw Mermaid `erDiagram` file from the active Spec Kit `data-model.md`, either after `/speckit.plan` or by manual invocation.

## Features

- Provides the `speckit.data-model-diagram.generate` command
- Registers an optional `after_plan` hook
- Reads the active feature `data-model.md`
- Writes `data-model-diagram.mmd` next to the source data model
- Preserves documentary field types instead of collapsing them to generic primitives
- Keeps Mermaid relationship lines and typed relationship-bearing attributes
- Encodes arrays and nullability with Mermaid-safe tokens such as `ActivityRecord_array` and `timestamp_nullable`

## Requirements

- Spec Kit `specify` CLI with extension support
- A Spec Kit project that runs `/speckit.plan`
- Spec Kit version compatible with `>=0.2.0`

## Installation

### From a GitHub Release

```bash
specify extension add data-model-diagram --from https://github.com/benizzio/spec-kit-data-model-diagram/archive/refs/tags/v0.2.0.zip
```

### Development Install

```bash
specify extension add --dev /home/benizzio/src-workspace/spec-kit-data-model-diagram
```

## Usage

1. Run `/speckit.plan` in your Spec Kit project.
2. The extension offers an optional `after_plan` hook that can run `speckit.data-model-diagram.generate`.
3. The command reads the active feature's `data-model.md` and overwrites `data-model-diagram.mmd` in the same feature directory.

You can also invoke the command manually from your coding agent after the extension is installed:

```text
/speckit.data-model-diagram.generate
```

## Output

The generated file contains raw Mermaid source only:

```text
erDiagram
    EntityA {
        UUID_string id
        decimal_string amount
        RelatedEntity_nullable related_entity
        ChildEntity_array child_entities
    }
    EntityA ||--o{ EntityB : contains
```

## Troubleshooting

- `data-model.md` missing: run `/speckit.plan` for the target feature first, or add the data model before invoking the command.
- Extension not available: run `specify extension list` and confirm `data-model-diagram` is installed.
- Command runs in the wrong place: execute it from the root of an initialized Spec Kit project so the core prerequisite script can resolve the active feature.

## Release Process

1. Update `extension.yml` with the new semantic version.
2. Add a matching entry to `CHANGELOG.md`.
3. Create and push a Git tag such as `v0.2.0`.
4. Create a GitHub release for that tag.
5. Submit the extension through the Spec Kit Extension Submission issue template.

Catalog submissions go through the issue template, not a direct pull request to the community catalog.
