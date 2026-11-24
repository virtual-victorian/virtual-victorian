---
name: blender-set-dressing
description: Place Blender props and environmental details for storytelling, composition, density, navigation clarity, and readable narrative clusters.
license: MIT
metadata:
  author: https://github.com/arjun988
  version: "1.0.0"
  domain: blender
  role: specialist
  triggers: set dressing, dressing, storytelling props, interior dressing, clutter, narrative placement, set decals
  related-skills: blender-prop-modeling, blender-scene-assembly, blender-environment-modeling, blender-horror-direction, blender-asset-qa
---

# Set Dressing

Story through placement. Controlled clutter. Readable clusters.

## When to Use

- Interior/exterior dressing passes
- Narrative prop clusters
- Avoiding empty or overstuffed spaces
- After modular layout is locked

## Workflow

```
Layout locked → Hero narrative props → Secondary clusters
  → Background filler → Decals/wear → Lighting check → QA
```

## Density Rules

| Zone | Density |
|------|---------|
| Hero interact zone | High intent, low noise |
| Midground | Supporting story clusters |
| Background | Sparse; silhouette only |
| Traversal path | Clearance for player/camera |

## Cluster Method

1. Pick a story beat (meal abandoned, workbench used, escape rushed)
2. Place 3–7 props that sell the beat
3. Add 1–2 wear/decal cues
4. Stop — do not fill every surface

## Prop Sources

- `blender-prop-modeling` for unique pieces
- Kitbash / modular libraries for filler
- Instance repeats; vary rotation/scale slightly

## Horror / Sparse Styles

- Fewer props, more meaning
- Negative space intentional
- Coordinate with `blender-horror-direction` / `blender-horror-direction`

## Constraints

### MUST DO
- Dress after greybox navigation works
- Keep player/camera paths clear
- Instance repeated meshes

### MUST NOT DO
- Uniform clutter wallpaper
- Block doors/windows without story reason

## Reference Guide

| Topic | Reference | Load When |
|-------|-----------|----------|
| Story Clusters | `references/story-clusters.md` | When needed |
| Density Map | `references/density-map.md` | When needed |
