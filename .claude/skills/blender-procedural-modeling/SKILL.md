---
name: blender-procedural-modeling
description: Create reusable procedural Blender assets and scatter systems with Geometry Nodes or modifier-driven generation. Use for rocks, roads, cables, vegetation, buildings, terrain, instancing, and parameterized node groups.
license: MIT
metadata:
  domain: blender
  role: specialist
---

# Blender Procedural Modeling

Choose the lightest procedural system that keeps the requested controls editable.

- Read [Geometry Nodes](references/geometry-nodes/guide.md) for node groups, fields, instancing, scattering, and reusable generators.
- Read [procedural modeling](references/procedural-modeling/guide.md) for deciding between Geometry Nodes, modifiers, curves, arrays, and repeatable asset recipes.

Use `blender-general-modeling` when the result is a one-off mesh with no useful parameters. Pair with `blender-environment-modeling` for level-scale integration and `blender-asset-optimization` before real-time delivery.

Preserve exposed inputs, deterministic seeds where reproducibility matters, named node groups, and a clear realization or bake plan for export.
