---
description: "Generate data-model-diagram.mmd from data-model.md via agent inference"
---

# Generate Data Model Diagram

Generate a Mermaid ER diagram for the active Spec Kit feature directory.

## Execution

1. Run `.specify/scripts/bash/check-prerequisites.sh --json --paths-only` from the repository root and parse the absolute `FEATURE_DIR`.
2. Read `${FEATURE_DIR}/data-model.md`. If it does not exist, stop and surface the error.
3. Infer the Mermaid `erDiagram` structure from the data model contents. Use the document's entities, fields, and relationships as the source of truth. Resolve gaps conservatively instead of inventing unsupported details.
4. Preserve the data model's declared field types as closely as Mermaid permits. Do not collapse concrete documentary types into generic primitives when the model already provides a more specific type such as `UUID string`, `decimal string`, `secret string`, `integer array`, `enum`, `timestamp nullable`, or a named entity type.
5. Mermaid ER attributes must use parser-safe type tokens. Represent arrays and nullability with descriptive underscore-based documentary types such as `ActivityRecord_array`, `timestamp_nullable`, `string_nullable`, or `ProtectedActivityCache_nullable` instead of `[]` or `?` suffixes.
6. Inside each Mermaid entity block, include not only scalar fields but also relationship-bearing attributes when they are explicitly present or directly implied by the data model fields and relationships. Use descriptive object or array types such as `SetupProfile setup_profile`, `ProtectedActivityCache_nullable protected_activity_cache`, or `AssetReportEntry_array summary_entries`.
7. When the data model expresses nullability or optionality, preserve that in the Mermaid attribute type using the same parser-safe documentary convention such as `timestamp_nullable`, `string_nullable`, or `NamedEntity_nullable`.
8. Keep the Mermaid relationship lines as well; the typed relationship-bearing attributes supplement them and do not replace them.
9. Write raw Mermaid source only to `${FEATURE_DIR}/data-model-diagram.mmd`, overwriting any existing file.
10. Report the absolute input and output paths.
