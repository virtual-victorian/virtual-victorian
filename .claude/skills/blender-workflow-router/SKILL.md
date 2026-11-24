---
name: blender-workflow-router
description: Route any Blender asset, scene, animation, rendering, or export request to the smallest useful set of Blender skills. Use first for multi-phase work, reference matching, or when the correct Blender specialty is unclear.
license: MIT
metadata:
  domain: blender
  role: orchestrator
---

# Blender Workflow Router

Choose a focused production stack before operating Blender. Do not load every related skill.

## Required operating standard

For asset creation or modification, read [the production standard](references/blender-tech-artist/guide.md) before writing Blender code. It contains the non-destructive modifier, material, UV, bake, safety, and render-inspection rules verified against the configured Blender environment.

For a narrow one-step request, use this router plus the one matching specialist. For a multi-phase project, add specialists only when their phase begins.

For manufacturer-photo/spec-driven industrial equipment and animated web GLBs, also use `industrial-equipment-from-reference` for provenance and mechanical/runtime gates; retain the primary-discipline and phase routing below.

## Routing model

1. Pick one primary discipline.
2. Add only the production-phase skills the deliverable needs.
3. Add at most one visual style, one world or mood, and one game-genre direction when those choices affect the asset.
4. Finish with optimization, export, and QA only when delivery requires them.

### Primary discipline

| Request | Lead skill |
|---|---|
| General blockout, cleanup, ordinary geometry | `blender-general-modeling` |
| Mechanical, manufactured, weapon, sci-fi | `blender-hard-surface-modeling` |
| Furniture, tools, household or hero props | `blender-prop-modeling` |
| Cars, aircraft, ships, mechs, transit | `blender-vehicle-modeling` |
| Buildings, terrain, modular levels | `blender-environment-modeling` |
| Trees, plants, grass, foliage | `blender-vegetation-modeling` |
| Human or humanoid characters | `blender-character-modeling` |
| Animals, monsters, aliens, fantasy creatures | `blender-creature-modeling` |
| Architectural presentation | `blender-architectural-visualization` |
| Procedural generation or Geometry Nodes | `blender-procedural-modeling` |

### Production phase

| Need | Skill |
|---|---|
| High-resolution surface form | `blender-sculpting` |
| Animation-ready topology | `blender-retopology` |
| UV seams, packing, UDIMs, lightmaps | `blender-uv-unwrapping` |
| Shaders and surface appearance | `blender-material-authoring` |
| Texture atlases and baked maps | `blender-texture-baking` |
| Final shading-read iteration | `blender-look-development` |
| Hair or fur | `blender-hair-grooming` |
| Garments and cloth | `blender-cloth-simulation` |
| Armatures and deformation controls | `blender-rigging` |
| Keyframed or NLA motion | `blender-animation` |
| Rigid body, soft body, destruction | `blender-physics-simulation` |
| Smoke, fire, particles, trails | `blender-visual-effects` |
| Large collection and set structure | `blender-scene-assembly` |
| Narrative prop placement | `blender-set-dressing` |
| Camera and shot language | `blender-camera-cinematography` |
| Illumination and atmosphere | `blender-lighting` |
| Cycles or Eevee output | `blender-rendering` |
| Render-pass finishing and grading | `blender-compositing` |

### Direction overlays

| Decision | Skill |
|---|---|
| Anime, low-poly, realistic, voxel, painterly, retro, or other visual treatment | `blender-style-direction` |
| Psychological, PSX, analog, folk, body, cosmic, liminal, or mascot horror | `blender-horror-direction` |
| Fantasy, sci-fi, historical, apocalyptic, or biome setting language | `blender-world-direction` |
| Cozy, gritty, brutalist, surreal, neon, or noir emotional treatment | `blender-mood-direction` |
| Shooter, RPG, survival, strategy, platformer, racing, or other gameplay readability | `blender-game-genre-direction` |
| Character role, race, silhouette, or body-type language | `blender-character-art-direction` |

### Delivery

| Need | Skill |
|---|---|
| Runtime cleanup and performance | `blender-asset-optimization` |
| Multiple mesh detail levels | `blender-lod-generation` |
| Game collision proxies | `blender-collision-meshes` |
| Generic, Unity, Unreal, or Godot delivery | `blender-export` |
| Final ship/no-ship inspection | `blender-asset-qa` |

## Common stacks

```text
Hero prop:
blender-workflow-router → blender-prop-modeling → blender-uv-unwrapping
→ blender-material-authoring → blender-look-development
→ blender-asset-optimization → blender-export → blender-asset-qa

Game character:
blender-workflow-router → blender-character-modeling → blender-sculpting
→ blender-retopology → blender-uv-unwrapping → blender-material-authoring
→ blender-rigging → blender-animation → blender-export → blender-asset-qa

Cinematic scene:
blender-workflow-router → primary discipline → blender-scene-assembly
→ blender-set-dressing → blender-look-development
→ blender-camera-cinematography → blender-lighting
→ blender-rendering → blender-compositing → blender-asset-qa
```

## Shared references

- Read [asset pipeline](references/core/asset-pipeline.md) when planning a multi-phase asset.
- Read [MCP integration](references/core/mcp-integration.md) before Blender MCP execution.
- Read [naming conventions](references/core/naming-conventions.md) when establishing scene structure.
- Read [polycount budgets](references/core/polycount-budgets.md) for game delivery.
- Read [validation checklist](references/core/validation-checklist.md) before delivery.
- For a supplied reference image, read [reference matching](references/core/reference-image-match.md), use the [analysis template](references/core/reference-analysis-template.md), and finish with the [visual-match checklist](references/core/visual-match-checklist.md).

The former director material is retained under `references/blender-director/`
for recovery only. Do not use it for routing; its historical skill names were
replaced by the tables in this entrypoint.
