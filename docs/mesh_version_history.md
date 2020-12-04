## Brigge for Meshes - version history

| Version | Release Date | Blender | Unreal |
|---|--:|---|---|
| [0.9.0](#090)  |  3 Dec 2020 | 2.80+ | 4.22 - 4.26 |
| [0.8.9](#089)  |  1 Aug 2020 | 2.80+ | 4.22 - 4.25 |
| [0.8.8](#088)  | 18 Jun 2020 | 2.80+ | 4.22 - 4.25 |
| [0.8.7](#087)  | 14 Apr 2020 | 2.80+ | 4.22 - 4.25 |
| [0.8.6](#086)  |  7 Apr 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.5](#085)  | 24 Mar 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.4](#084)  | 13 Mar 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.3](#083)  |  2 Mar 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.2a](#082a)| 21 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.2](#082)  | 17 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.1](#081)  | 14 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.0](#080)  | 31 Jan 2020 | 2.80+ | 4.22 - 4.24 |

---
### 0.9.0

Now ready for Unreal Engine 4.26 and beyond!

This release is smarter about naming conventions. For example, it knows `M_Leafy_Green` is the same as your `Leafy Green` material in Blender. When importing meshes to Unreal, Brigge finds materials and related assets by name. Now it is more likely to connect the right assets no matter which additional software is in your pipeline.

Lightmap UVs are now generated during import.

---
### 0.8.9

Brigge has an all-new parser and file reader on the Unreal side. It now uses UTF-8 throughout the pipeline, which means better support for international text and symbols. For example: you can say _rotation = 30°_ instead of _30 degrees_. And keep your original asset names no matter which language they're written in.

Reimport works better than ever. We fixed some bugs then kept going! Reimport can now handle:
* object position & scale updated in the scene
* material assignment changes
* added or removed LODs

There's a [new option](mesh_options.md) to export tangents from Blender in addition to normals.

Modest speed improvement during export and import.

#### Bug Fixes
_Reimport mesh assets that don't have LODs (regression from 0.8.8)._

_Handle actor name conflicts gracefully, i.e. don't crash Unreal._

_Fixed material assignment when list of material names is longer than the asset's slot count._

---
### 0.8.8

Create several **levels of detail** for each mesh asset, and Brigge collects them into a single game asset that renders efficiently at any distance. It uses a simple and familiar naming scheme on the Blender side, and otherwise lets you organize the project however you want! See the new [LOD docs](mesh_lod.md) to learn more.

Placing actors in the level gets a **massive speed boost** in this update.

We rewrote the way Quads and N-gons are imported to Unreal. The visual result should be the same (final triangles rendered by the GPU) but the topology is cleaner. Ready for further editing. Each polygon matches _exactly_ between Unreal and Blender now.

#### Bug Fixes
_Quads would sometimes split across the wrong diagonal in Unreal 4.24 and 4.25. It now works correctly for all engine versions._

_Parts of N-gons could have the wrong material in Unreal 4.24 and 4.25. Fixed!_

---
### 0.8.7

Exporting models from Blender is *much faster* now. We profiled the exporter code and rewrote most of it with performance in mind. Why? **Iteration times.** When you can see your work in the game quickly, it's easier to make a few more changes before calling it "done", leading to a higher quality bar for the whole project.

A new *"Optimize Exported Meshes"* option lets you decide whether to spend extra time reducing file size between Blender and Unreal. Earlier versions of Brigge *always* optimized files. This helps when storage space or network bandwidth are limited.

Compared to version 0.8.6 we're seeing **2-3x** faster export of optimized models, and around **6x** for unoptimized. For fastest iteration times, leave this option unchecked.

---
### 0.8.6

This release unifies how asset names and filenames are handled between our Blender and UE4 plugins, continuing what we started in 0.8.4. StaticMesh assets, Materials, and Actors keep their original Blender names in the .brigge file.

For example, this asset:
```
mesh named "Cat's House" at (0, 0, 0) {
  data = 'Tall Cat House'
  # StaticMesh will be 'Tall_Cat_House' in Unreal
  material = 'Material X'
  # 'Material X' will be 'Material_X' in Unreal
}
```
will produce a StaticMesh named `Tall_Cat_House` from data stored in the file `Tall Cat House.mesh` and spawn an Actor named `Cat's House` in Unreal. If a material named `Material_X` is already in your project, it will auto-fill `Cat's House` material slot.

Also, several warnings are cleaned up, to let you know *exactly* what issues Brigge found during export.

#### Note
We changed how names are written & interpreted, so you'll need to re-export older .brigge files from Blender.

---
### 0.8.5

Brigge now preserves **parent-child relationships** between objects in your scene. When importing to UE4, Actors will attach to their parent Actor whether it's also in the current import or already in your level.

Actor labels in UE4 match their original mesh object's name in Blender. (*Actors don't have the same name restrictions as StaticMesh or other assets.*)

Brigge lets you know when a mesh has no geometry or has any unused material slots. Find this information in Blender's console and in the exported .brigge file.

We removed the "Degrees" option from the Blender exporter to keep the UI simple. Brigge will export rotations as degrees or radians based on Blender scene settings.

We added an option to write detailed mesh statistics during export. Tech artists can use this info to track their models against polygon budgets for example. It's also useful for version control to see what *exactly* changed between two versions of your asset instead of simply *"it changed"*.

Plus some small changes to make the .brigge format more human friendly!

---
### 0.8.4

Blender and Unreal have different standards for naming assets, and different restrictions on *what those names can include*. Brigge will now warn you when something must be renamed when imported into Unreal. Look in Blender's console or in the exported .brigge file for this info:
```
mesh named 'Cube.001' at (-2.48867, -3.34452, -0.404826) {
  # Cube.001 will be Cube_001 in Unreal
  data = 'Cube.mesh'
  scale = 0.228686
}
```

Brigge will also warn about using filenames that cause problems on cross-platform projects. Finding these problems early will help people collaborate on your project, whether they use Mac, Linux, or Windows.
```
# Aux.mesh is not a safe cross-platform filename. How about Aux_.mesh ?
```

The .brigge format can now have single quotes / apostrophes inside text strings. `"Like 'this'!"` It also works `'The "other" way'`.

We taught the Unreal plugin how to read very small and very large numbers, using exponential notation. Our Blender addon uses Python to write these, and now everything works end-to-end.

---
### 0.8.3

This release is all about **instancing** and **reuse**!

With Brigge you can layout a scene in Blender and bring that scene into Unreal all at once.

This update enables you to reuse mesh data in Blender (using *"duplicate linked"*) and reuse StaticMeshes on the UE4 side. Multiple Actors can share each StaticMesh asset, while having their own position, rotation, and scale. Actors can even override their StaticMesh's materials to bring variety to your scenes.

Exporting a scene with lots of sharing/reuse is much faster now, and produces fewer output files. Importing the scene into UE4 is also much faster.

---
### 0.8.2a

> *Plugin 'BriggeMesh' failed to load because module 'BriggeMesh' could not be loaded. There may be an operating system error or the module may not be properly set up.*

We hope you never saw that error message! But if you did we have good news: we fixed the problem by updating our build system. Here are the freshly built plugins for Unreal!

Everyone should update to this latest version.

#### Notes
1. The new plugin version is 0.8.2a. It has the same features and file format as 0.8.2, but should run on more systems without trouble.
2. The Blender add-on did not need changes. If you already have version 0.8.2 you can keep using it.

---
### 0.8.2

We changed how scale is applied to objects. When you let Brigge place objects into the level, their scale is applied to each StaticMesh Actor. If you choose not to place meshes, scale is applied to the StaticMesh asset itself. You can then place these into the level and they'll be the correct size.

Rotations are now written for people to understand. Brigge supports multiple forms of rotation, and in this release we revamped how axis-angle rotations are described.

For example you can use radians

> rotation = 0.692073 radians about (0, 0, 1)

or degrees

> rotation = 30 degrees about (1, 0, 0)

or axis names

> rotation = 30 degrees about 'X'

also shorthand units

> rotation = -15 deg about 'Y'

even omit the axis to simply specify a lateral direction

> rotation = 135 degrees

#### Notes
1. For objects with *negative* scale, Blender and Unreal both do the right thing: mirror the geometry and draw it right-side-out. Brigge keeps this working with *placed* objects, but *unplaced* StaticMesh assets will display inside-out in UE4. You could apply object scale in Blender to avoid this.
2. Brigge uses Blender conventions, so rotation about the 'X' axis means Blender's +X axis.
3. Omit the rotation axis to use "up" which is +Z in both Blender and Unreal. This is most useful for turning a model in a certain direction, without tilting it.

---
### 0.8.1

From the Blender exporter you can choose to "Place Objects", and Brigge will remember the position and rotation of each object. With this update these objects will be placed into your Unreal level. Now you can build a complete scene in Blender and bring it into your UE4 project in one step!

#### Notes
1. In future updates we plan to offer more ways to control exactly what is imported. But we wanted to get this new feature into your hands right away!
2. We changed the way rotations are stored, so you'll need to re-export old .brigge files from Blender.

---
### 0.8.0

First public *Early Access* release. We started developing Brigge for Meshes in March 2019 and it seemed about 80% *"Done"* (whatever *that* means) when we started offering it for sale.
