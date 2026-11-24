<!-- Consolidated from the former `blender-workflow-router` skill. -->


# Blender Tech Artist — Operating Standard

Ship assets the way a technical artist does: **operators and modifiers, never hand-plotted
vertices; procedural shading, never flat colour; measured output, never assumed output.**

Everything below is implemented and tested in a local `bltech.py` helper module (not included in this repo).
**Import it rather than re-deriving the code.**

```python
import sys; sys.path.insert(0, "path/to/blender-work")
import bltech
```

## The Five Hard Rules

Apply these unless the art direction explicitly says otherwise (voxel/PSX/flat-shaded
Roblox work is the standing exception — see *Roblox Exception*).

1. **Every hard edge gets a Bevel modifier.** 0.01–0.02 world units, 2 segments,
   ANGLE limit ~42°. Edges that catch light are the single biggest realism cue.
   Never bevel in Edit Mode — the modifier stays editable.
2. **Every surface gets micro-variation.** A Principled BSDF with a constant
   roughness reads as plastic. Minimum: Noise → ColorRamp → Roughness.
3. **Every mesh gets UVs before texturing or baking.** `smart_project` →
   `average_islands_scale` → `pack_islands`. Skipping the equalise step is why
   auto-UVs bake blotchy.
4. **Every asset gets audited before export.** `bltech.report()` — applied scale,
   manifold, no loose verts, UVs present, tri budget.
5. **Every pass ends by looking at a render.** `bltech.hero_shot()` then actually
   Read the PNG back. Asserting an asset looks right without opening the image is
   how flat-grey blockouts ship.

## Modifier Stack — the only order that shades correctly

```
Mirror → Array → Solidify → Boolean → Bevel → Subsurf → WeightedNormal → Triangulate
```

`bltech.finish(obj, bevel=0.015, segments=2, subsurf=0, solidify=0.0,
weighted_normal=True, smooth_angle=32)` builds this and applies scale first.

**Apply scale before Bevel.** Non-uniform object scale produces uneven bevel width —
that is the "why is one edge fat" bug.

**Never combine Weighted Normal with Smooth-by-Angle.** They are competing normal
solvers, and Blender pins "Smooth by Angle" to the *end* of the stack where it
overwrites the weighted normals. Measured on a beveled cube, deviation of flat-face
split normals from the face normal:

| Config | Deviation | Result |
|---|---|---|
| Sharp-edge marking + Weighted Normal | **0.000°** | flat panels stay flat |
| Smooth by Angle | 14.694° | panels smear near bevels |
| Plain shade_smooth | 14.694° | same artifact |

`finish()` picks the right one: `weighted_normal=True` marks sharp edges by angle and
adds the Weighted Normal modifier; `weighted_normal=False` uses Smooth by Angle
(the organic default).

Keep booleans **live** until export prep. Keep the whole stack live — apply only in
the export script, never in the build script.

## Materials — `bltech.pbr_material()`

| Arg | Range | What it buys |
|---|---|---|
| `micro` | 0–0.3 | Roughness breakup. **Highest value-per-node knob.** ~4 nodes, kills the plastic look. |
| `wear` | 0–1 | Bevel-node curvature mask → lightens and polishes catching edges |
| `bump` | 0–0.05 | Surface relief in metres, zero geometry cost |
| `mottle` | 0–1 | Voronoi tint drift for dirt, oxidation, organics |
| `preset` | see `PBR` | Physically sane metallic/roughness pairs |

Use `bltech.P[...]` for socket names — see *API deltas*, the old names are gone.

Keep the master-material discipline from the `blender-material-authoring` skill: few masters, many
parameter variations. `pbr_material(reuse=True)` is idempotent across re-runs.

## UVs and texel density

`bltech.auto_uv(objs)` then `bltech.texel_density(obj, texture_px)` returns measured
**px per metre** — so texture resolution is a decision, not a guess. Bump the atlas
size until density lands in range rather than eyeballing it.

Packing normalises absolute density into 0–1 space; consistency *between islands* is
what `average_islands_scale` guarantees, and that is what matters for a clean bake.

## Baking — mandatory for Roblox

A procedural node tree does not exist outside Blender. Any engine that will not
evaluate it needs the shading **baked to an image**:

`bltech.bake_atlas(objs, out_path, size=1024)` — Cycles DIFFUSE/COLOR, restores the
previous engine. **EEVEE cannot object-bake.** Without this step, every bit of
`micro`/`wear`/`mottle` work is invisible in-game.

The proven end-to-end Roblox path already exists in `sawfish_export_v09.py`:
per-unit BakeUV → smart_project → pack into an N×N atlas tile → Cycles bake → PNG →
FBX + manifest → `rbxupload.py`. Follow it; do not invent a second one.

## Roblox Exception

The delivery target is usually Roblox, not a film renderer. Adapt:

- **Flat-shaded voxel/blocky assets** (`roblox-blocky-rig` work): use
  `bltech.faceted()`, not `finish()`. Faceting is the art direction. Bevel and
  Subsurf are wrong there.
- **Mesh assets** get the full `finish()` treatment, then bake, then export.
- **Subsurf is a budget risk.** Roblox tri budgets are tight — check
  `bltech.report(tri_budget=...)` with the *evaluated* mesh, which is what
  `report()` measures.
- Roblox reads one albedo texture per MeshPart (plus optional SurfaceAppearance
  maps). Bake to that, don't ship node trees.
- 1 unit = 1 stud in this project's blend files.

## QA loop — non-negotiable

```
build → finish() → material → auto_uv() → report() → hero_shot() → READ THE PNG → fix
```

`hero_shot()` auto-frames from the real camera FOV, adds three-point area lights
scaled to the subject, sets AgX, and writes a PNG. Read it with the Read tool.
Repeat up to 3 passes; if it still reads wrong, the problem is proportion or
silhouette, not shading — go back to blockout.

## Add-on reality check (verified 2026-09)

Enabled in this Blender: **FBX, glTF, BVH, SVG, UV-layout, pose library, Lab MCP,
RBX Animation Importer.**

Not installed: **Node Wrangler, Rigify, Dream Textures, Auto-Rig Pro, fSpy.**

- **Node Wrangler** is UI-only with no useful headless API. Its scripted equivalent
  is `bltech._n()` + explicit `nt.links.new()`. Do not tell the user to script it.
- **Rigify** ships with Blender and is one checkbox away if a humanoid rig is needed.
- **Dream Textures / Auto-Rig Pro / fSpy** are third-party downloads. Do not assume
  they exist. If a task genuinely needs one, say so and let the user install it.
- Texture generation today goes through **Roblox Studio's `generate_texture` /
  `generate_material`** or a baked procedural, not a Blender AI add-on.

## API deltas — Blender 5.2.1 (old snippets are wrong)

| Stale | Current |
|---|---|
| `"Specular"` | `"Specular IOR Level"` |
| `"Emission"` | `"Emission Color"` + `"Emission Strength"` |
| `"Subsurface"` / `"Transmission"` / `"Sheen"` | `"… Weight"` |
| `mesh.use_auto_smooth` | **removed 4.1** → `bpy.ops.object.shade_auto_smooth(angle=)` |
| `ShaderNodeTexMusgrave` | **removed 4.1**, folded into `ShaderNodeTexNoise` |
| `ShaderNodeMixRGB` (`Color1`/`Color2`/`Fac`) | `ShaderNodeMix` (`A`/`B`/`Factor`/`Result`) |
| `bpy.ops.object.bake` in EEVEE | Cycles only — set the engine first |

`RenderSettings.engine` introspects as EEVEE-only; Cycles registers dynamically, so
`scene.render.engine = 'CYCLES'` works regardless of what the enum lists.

## Execution notes

- **Never destroy the user's open scene.** Blender usually has unsaved work loaded.
  Test and iterate in a headless instance:
  `/Applications/Blender.app/Contents/MacOS/Blender --background --factory-startup --python script.py`
- The MCP socket path (`python3 blsend.py script.py`) does not return stdout —
  **write results to a JSON file and read that back.**
- Set active **and** selection before every operator; operators clobber both.
- `bltech.sync()` before reading evaluated geometry or world matrices.
