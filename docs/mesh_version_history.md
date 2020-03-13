## Brigge for Meshes - version history

| Version | Release Date | Blender | Unreal |
|---|--:|---|---|
| [0.8.4](#084)  | 13 Mar 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.3](#083)  |  2 Mar 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.2a](#082a)| 21 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.2](#082)  | 17 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.1](#081)  | 14 Feb 2020 | 2.80+ | 4.22 - 4.24 |
| [0.8.0](#080)  | 31 Jan 2020 | 2.80+ | 4.22 - 4.24 |

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
