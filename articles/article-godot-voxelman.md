```
#
# @file   : article-godot-voxelman.md
# @version: 2021-02-10
# @created: 2021-02-08
# @author : pyramid
#
```

<header>
<p style="font-weight:600; font-size:36px">Godot Voxel Engine</p>
<p style="font-weight:600; font-size:32px">libProcU - Procedural Universe Library</p>
</header>


--------------------------



**Introduction**
=====================================

[Voxels](https://en.wikipedia.org/wiki/Voxel) are three dimensional pixels used to construct volumetric models.

Although modern computers are better equipped to rendering polygons, voxels are easier to comprehend and extend with advanced modeling techniques because they resemble more our concept of how matter behaves (e.g. adding materials by depositions, removing materials by excavation).



**Advantages** of using voxels

- more accurate building blocks because they mimic particles
- unlock new simulation techniques that would be impossible with other modeling methods (e.g. fluid simulation)
- are quicker in modeling and visualization of volumetric data (especially in natural or organic formations)
- Procedural generation of terrain and objects
- In game modifiability and destructibility of terrain and objects
- Meshes only model surfaces - an incomplete and ‘hollow’ approximation of reality. Voxels can represent the full depth of matter; a fundamentally more accurate model of reality
- Mesh content creation is complex and technically demanding; costly with high barriers of entry. Content creation with voxels is direct and straightforward, like working in clay. Children can do it (see *Minecraft*)
- Many layers of “hacks” (e.g. UVs) make editing and distributing mesh assets prohibitively cumbersome. Uniform and simple voxel structure allow for granular level-of-detail and powerful scaling



**Drawbacks** of voxel engines

- coarseness and rasterization of model requires additional processing to create a continuous smooth image
- modeling technologies to build complex objects are not yet so advanced
- hardware is not specialized for efficiently rendering high-resolution voxels
- Large scenes made out of millimeter-scale voxels presents a difficult **data management** problem






**Goals and Requirements**
=====================================

Requirements for procedural universe content using voxels:

**Procedural** content shall be generated on-the-fly. To speed up revisiting, content may be stored and retrieved so that it does not need to be rebuilt. This is especially the case for multiplayer environments.

**Diversity** enables immersion and believability. This means that biomes should not repeat too often on the same planet. 

**Procedural Terrain** is our first goal, because without a world, there can be no game.



State of Technology
-------------------------------------

The paper "Procedural feature generation for volumetric terrains using voxel grammars" by Rahul Dey et al, 2018 lists technologies for voxel management: Sparse Voxel Octree (SVO), VDB (Volumetric Dynamic B+) trees, adaptive dual contouring, storing extra information in voxels.





**Findings**
=====================================



**Installing the Godot Engine**
-------------------------------------

Clone the source code repository
```bash
> git clone https://github.com/godotengine/godot.git
> cd godot.git
```

Follow the compilation instructions for your operating system and distribution
```
https://docs.godotengine.org/en/latest/development/compiling/
```

Compile the Godot engine
```bash
# compile with all cores
> scons -j$(nproc) platform=linuxbsd
# compile with less one core
> scons -j$(($(nproc) - 1)) platform=linuxbsd tools=no target=release bits=64
# other optimization parameters
tools=yes use_lto=yes
# to see all parameters
> scons --help
```


After compilation, test the engine by starting it
```bash
# show available binaries
> ls -l bin
# run godot
> bin/godot.linuxbsd.opt.64
```




**Installing the Voxel Engine**
-------------------------------------

Follow the compilation instructions from the source repository
```
https://github.com/Relintai/voxelman
```


Clone the source code repository

```bash
> cd godot/modules
> git clone https://github.com/Relintai/voxelman.git
> git clone https://github.com/Relintai/texture_packer.git
> git clone https://github.com/Relintai/mesh_data_resource.git
> git clone https://github.com/Relintai/thread_pool.git
> git clone https://github.com/Relintai/props.git
> git clone https://github.com/Relintai/mesh_utils.git
```

Recompile the Godot engine
```bash
# voxel engine must be compiled with tools
> scons -j$(($(nproc) - 1)) platform=linuxbsd target=release_debug bits=64
```

Test the compiled engine
```bash
> bin/godot.linuxbsd.opt.64
```



**Demo Games**
-------------------------------------

**The Tower** - a simple demonstration project

```bash
> git clone https://github.com/Relintai/the_tower.git
```



**Broken Seals**, an old skool multiplayer RPG

```bash
> git clone https://github.com/Relintai/broken_seals.git
```

This game requires additional modules for the Godot editor
```bash
> cd godot/modules
> git clone https://github.com/Relintai/world_generator.git
> git clone https://github.com/Relintai/entity_spell_system.git
> git clone https://github.com/Relintai/ui_extensions.git
> git clone https://github.com/Relintai/godot_fastnoise.git fastnoise
> git clone https://github.com/Relintai/procedural_animations.git
> git clone https://github.com/Relintai/broken_seals_module.git
```

Recompile and test the Godot engine
```bash
> scons -j$(($(nproc) - 1)) platform=linuxbsd target=release_debug
> bin/godot.linuxbsd.opt.64
```

Or clone the complete Godot engine and dependencies into the ```engine``` directory and build the game
```bash
# create engine
> scons
# compile and run game for Linux
> scons bel -j$(($(nproc) - 1))
> ...
# compile game for Android
> scons bea -j$(($(nproc) - 1))
```






**Conclusion & Future Work**
=====================================

**The Tower** is a platform jumping game constructed of free floating boxes that you have to navigate and jump on without falling down. It s a simple game with nice graphics but does not contribute to the intended target.

**Broken Seals**, is a simple RPG world without fancy graphics. It generates LODed chunks of voxel terrain with a simple noise function and uses a **Marching Squares** algorithm to texture polygons with some texture blending.

**Voxel Engine** (Godot) is used in **Broken Seals** to generate procedural voxel terrain. It is a good starting point for further extension of the algorithm for Godot.






**References**
=====================================

[1]  Godot Game Engine. https://godotengine.org/.

[2]  Godot Engine Source Code Repository. https://github.com/godotengine/godot.

[3] Godot Engine official documentation. https://github.com/godotengine/godot-docs.

[4] A curated list of free/libre plugins, scripts and add-ons for Godot. https://github.com/godotengine/awesome-godot.

[5] Unofficial Godot Engine and Documentation Builds. https://hugo.pro/projects/godot-builds/.

[6] A voxel engine module for Godot. https://github.com/Relintai/voxelman.

[7] The Tower - a Godot voxel engine demonstration project. https://github.com/Relintai/the_tower.

[8] Procedural feature generation for volumetric terrains using voxel grammars. Rahul Dey et al. 2018. http://eprints.bournemouth.ac.uk/31174/1/1-s2.0-S1875952117301349-main.pdf.

[9] VDB: High-resolution sparse volumes with dynamic topology. [Ken Museth](https://www.semanticscholar.org/author/Ken-Museth/2854269). bin2013. http://www.museth.org/Ken/OpenVDB_files/Museth_TOG13.pdf.

[10] Voxelman Engine for Godot. https://github.com/Relintai/voxelman. [Péter Magyar](https://github.com/Relintai) ([Relintai](https://github.com/Relintai)). Retrieved 2021-02-08.

