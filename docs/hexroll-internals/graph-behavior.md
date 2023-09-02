# hexroll-entities-graph

To better understand how this version of Hexroll handles changes in the entity graph, let's take a look at the following example.

## Removing a dungeon

### Realm is being generated
During the early stages of generating a realm, Hexroll will have a set of empty hexes generated.
![Step1](/images/graph-1.png)

The first layer of hex features is going to be dungeons. In this stage, Hexroll will begin adding random dungeons to random hexes.
![Step1](/images/graph-2.png)
And then another one.
![Step1](/images/graph-3.png)

In later stages, Hexroll will begin adding other features, such as cities with NPCs.
![Step3](/images/graph-4.png)

And together with NPCs, Hexroll will begin generating quests. Some quests will rely on locations generated in earlier stages.
![Step4](/images/graph-5.png)

### Dungeon is removed

If for while editing the map after quests were already generated, a location used by a quest is invalidated, Hexroll will try resolving the inconsistency by finding an alternative location for the quest. This same principal applies to other entity types throughout the sandbox.
![Step5](/images/graph-6.png)

### The last dungeon is removed

If following entity invalidation, an entity relation with another entity could not be reconstructed, it may invalidate the other entity as well.
![Step5](/images/graph-8.png)

### A new dungeon is added

Even if a new dungeon is generated, the invalidated quest is gone forever, and you will have to regenerate a new quest for the NPC
![Step5](/images/graph-9.png)

