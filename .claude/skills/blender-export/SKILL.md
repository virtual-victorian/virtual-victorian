---
name: blender-export
description: Export Blender assets to FBX, GLTF/GLB, OBJ, USD, Alembic, Unity, Unreal Engine, or Godot with correct scale, axes, pivots, materials, animation, LODs, and collision. Use only for delivery or import troubleshooting.
license: MIT
metadata:
  domain: blender
  role: specialist
---

# Blender Export

Select exactly one target guide, then validate the imported result in the destination when that tool is available.

| Target | Read |
|---|---|
| Generic FBX, GLTF/GLB, OBJ, USD, Alembic | [generic export](references/export-pipeline/guide.md) |
| Unity | [Unity export](references/unity-export/guide.md) |
| Unreal Engine | [Unreal export](references/unreal-export/guide.md) |
| Godot | [Godot export](references/godot-export/guide.md) |

Before export, use `blender-asset-optimization` when runtime budgets matter, `blender-lod-generation` when multiple detail levels are required, and `blender-collision-meshes` when the engine needs authored collision. Finish with `blender-asset-qa`.

Do not apply irreversible transforms or modifiers without preserving a recoverable source. Confirm format, units, up/forward axes, pivot, animation scope, and material limitations before writing the export.
