```
#
# @file   : article-random-hierarchies.md
# @version: 2021-02-10
# @created: 2020-02-18
# @author : pyramid
#
```

<header>
<p style="font-weight:600; font-size:36px">Random Seed Hierarchies</p>
<p style="font-weight:600; font-size:32px">libProcU - Procedural Universe Library</p>
</header>

--------------------------


**Introduction**
=====================================

Procedural content generation in space games has been around since the early game history starting with Elite (1984) or Star Flight (1986).

The Procedural Universe Library (libProcU) facilitates creating procedural content in liberated games.

This article discusses the basic setup of random seeds for procedural content based on the example of a planet with different biomes.



**Goals and Requirements**
=====================================

There are two seemingly contrary requirements for procedural universe content.

**Reproducibility** of content is important in multiplayer environments (every player's client will generate the same content for a given starting seed) or when loading previously saved progress (in procedural single-player games).

**Uniqueness** of content when given a different starting seed.

**Seed Hierarchy** is necessary when we create subcontent for a seeded parent, e.g. a biome for a given planet. Where the parent planet seed has been given, the child seeds for biomes should depend on the planet's seed.

**Sequence Independence**. Another aspect of randomness is that for hierarchical seed populations. The creation of child seeds should be independent of creation order, i.e. biome 1 would always have the same seed, independent of the order in which it was created. This may be necessary when first creating those biomes where the player begins his exploration before creating other biomes as would be the case in sequential generation.



**Findings**
=====================================

For those requirements, we propose the following mechanisms.

We take the following example structure hierarchy:

- Planet which contains

  - Biome 1
  - Biome 2


The overall flow for this example structure is


- create new unique master_seed
- create biome 1 and 2 seeds derived from master_seed 
- continue for substructures of biome 1 and 2 but derived from biome seeds
- use random number sequence on the lowest hierarchy level. here, individual seeds are not needed anymore




**Uniqueness**
-------------------------------------

We start with a random number to create the initial game seed:

```
  using namespace std;

  // create pristine seed
  cout << "--- creating pristine random seed:\n";
  random_device rd; // rom random device
  uint64_t uSeed = static_cast<uint64_t>(rd());
  cout << "  seed from random device   : 0x" << hex 
	<< setw(16) << setfill('0') << uSeed
    << dec << " (" << uSeed << ") ("<< sizeof(uSeed) << " bytes)\n";

```



**Reproducibility**
-------------------------------------

There are two fundamental approaches to reproducibility:

- Generate the content once and store and reload it. It may be a feasible approach where the amount of data is not huge.

- Where large data sets are involved (imagine terrain data for a whole planet), the approach must be by derived seed hierarchy from a master seed (pristine upon first generation, reloaded upon subsequent generations).

When saving a single-player game, we must save the master seed, and reload it upon loading the saved game state.

```
// when saving
save(master_seed);

// when loading
uint64_t master_seed = load(master_seed);

```



**Seed Hierarchy**
-------------------------------------

Once the initial random master seed has been created, the subseeds must be then calculated deterministically so as to assure reproducibility.

A simple approach is to create a linear function for subseeds and add the master seed to it.

```
// very simple subseeding
for (int seed_number=0, i<2, i++) {
  seed[seed_number] = master_seed + seed_number;
}

```



**Sequence Independence**
-------------------------------------

Where substructures depend on several parameters, we may reserve specific ranges for each parameter. for two parameters in the range 0-99, this approach may be useful to avoid repetition and guarantee Independence of the sequence in which content is generated.

That is, the seed for a given tuple (param_a, param_b) can be immediately reproduced and does not require the whole sequence of seeds (e.g. for all biomes) to be generated:

```
// subseeds based on two patameters
void subseed(int param_a, int param_b) {
  subseed[] = master_seed + param_b * 1e2 + param_a;
}

```



**Conclusion & Future Work**
=====================================

We have demonstrated the simplicity of procedural generation approach guaranteeing to fulfill the four fundamental requirements:

- Reproducibility
- Uniqueness
- Seed Hierarchy
- Sequence Independence

In future, we want to apply the presented principles to generating whole galaxies for fictional universes.




**References**
=====================================

[1]  Procedurally Generating an Artificial Galaxy. Olof Elfwering. 2016.

[2]  PCG: A Family of Simple Fast Space-Efficient Statistically Good Algorithms for Random Number Generation. Melissa O'Neill. 2014.

[3]  PCG32 - a tiny self-contained C++ implementation of the PCG32 random number. Wenzel Jakob (wjakob). 2016. https://github.com/wjakob/pcg32. Retrieved on 2020-02-20.


