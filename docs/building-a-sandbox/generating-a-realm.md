# Generating a Realm

## The Generator Settings Window

When you first visit a new sandbox, you will be presented with the realm generator window.

This is the spinner. When you see the spinner, it means Hexroll is now processing a request on the server side. If you see the spinner for more than 45 seconds, it means something went wrong with the last request - usually a server-side error. In most cases, simply refreshing the page should get things back in order.

Pressing the `GENERATE` button will tell the generator to use all the parameters from the window and generate a new realm.

!!! note

    Read more about how Hexroll fantasy realms generator work internally here.

![Generator Page](/images/generator-generate.png)

Let's go through the different sections in this window and see how we can configure the generator in different ways.

### Realm Template

Changing the realm template options will modify most or all other parameters in the window using a predefined setting.

There are two drop-down lists to choose from:

![Templates](/images/generator-templates.png)

**Realm Type**:
Choosing a realm type will change the scale of the realm and will affect the name and the ruling structure of the realm.

**Population**:
You can choose to start with a fully populated realm, or choose to manually roll for settlements by editing the map.

!!! note

    If you choose to start with an unpopulated realm, you may also want to manually add factions to your realm after enough NPCs are generated.

### Terrain

Each realm is composed of a set of regions. Each region can have one or more hexes of the same biome type. You can have fewer regions that span over large areas, or many smaller regions. You can even create a single region with a single hex.

![Terrain](/images/generator-terrain.png)

**Number of Regions**:
Hexroll regions are named stretches of land sharing the same biome. When rolling a realm, Hexroll will generate a random number of regions between the specified values.

To determine which types of biomes to use, Hexroll will use the set probability multipliers `Px`. The higher the value is in comparison to other biomes, the more likely a biome will be randomly selected.

Each biome region can have a different hex count, depending on the set minimum value `>=` and maximum value `<=`.

### Dungeons

Dungeons are generated following the terrain. Dungeons provide an important infrastructural element for Hexroll realms. Dungeons provide essential building blocks that are later used by randomly generated quests and plot hooks.

![Dungeons](/images/generator-dungeons.png)

**Number of Dungeons**:

Hexroll will generate a random number of dungeons between the set values here.

**Dungeon Types Probability**:

Hexroll will use the set probability multipliers `Px` for each one of the dungeon types: Caves, Temples, and Tombs.

**Dungeons Size:**

Toggling this will make the generated dungeons generally smaller.

**Wandering Monsters**:
The list of wandering monsters inside a dungeon will be limited to include monsters with an HD equal or lower than the value specified here.

### Dwellings

Dwellings are the first generated layer of populated hex features in Hexroll. Dwellings can be small cabins
and farms, or larger strongholds hosting higher level NPCs such as castles and wizard towers.

![Dwellings](/images/generator-dwellings.png)

### Inns

Inns are the second level of populated hex features in Hexroll. Inns can provide characters with a safer place to stop at in their journeys and contain a good number of NPCs to ask for rumors or hire.

![Inns](/images/generator-inns.png)

### Settlements

Settlements are the third level of populated hex features. Settlements add shops, services and taverns together and usually contain a large number of NPCs to interact with. Some NPCs will have randomly generated plot or quest hooks that link to other places in the sandbox.

![Settlements](/images/generator-settlements.png)

**Number of Settlements**:
Hexroll will generate a random number of settlements between the set values here.

**Settlement Types Probability**:
Hexroll will use the set probability multipliers `Px` for each settlement type: Cities, Towns and Villages.

### Factions

Factions are yet another good source for story hooks and NPCs motivation. Factions use already generated NPCs as members and collaborators, except for the faction leader who is generated together with the faction entity.

While factions do not have a unique location on the map, they are associated with one of the pre-generated entities - usually a dungeon or a tavern.

![Factions](/images/generator-factions.png)

### Scroll Mods

![ScrollMod](/images/generator-mods.png)

Hexroll allows modifying and adding to its existing generator rules using SCROLL - a dedicated language designed specifically for Hexroll.

Read more about modding in the [Modding Guide](/scroll/scroll-mods/).

### Tweaks and Experiments

This is where you can find experimental settings, usually following incoming requests from the community.

![Tweaks](/images/generator-tweaks.png)

**Treasure Factor** is an experimental tweak acting as a multiplier for any treasure in the sandbox. This factor is applied post-generation and during page rendering. It is useful if you play other systems where treasure is scaled down.

### Behind the scenes

Hexroll is not a hex map editor. It is first and foremost a bidirectional sandbox generator. Adding and removing hexes or hex features in Hexroll has a cascading effect. If for example you remove a city, Hexroll will need to ensure other entities in the world are aware that all the NPCs, quests, rumors etc. removed from the sandbox are gone and consolidate the content accordingly.
