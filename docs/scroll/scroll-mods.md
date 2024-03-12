# Modding Hexroll using Scroll
[DRAFT]

## Overview

You can use scroll to change or extend the default random generation rules that hexroll uses.
This can be done by creating or using a scroll module with a unique link and then feeding the scroll unique identifier from the link into the realm generation window:

This guide specifies the current set of entities that you can extend or change together with an explanation for how to do so.

In most cases, the explaination will be accompanied with a usable example. You can use any example provided as is, or clone the scroll and modify it to your liking.

## Key Concepts

### Starting point

Scroll can be used to create a stand-alone random generator, but creating a sandbox mod requires basing it on the existing OSR generator model.

To do so, all you need to do is import the `OSR2` scroll like so:

```
# the following line will import the entire sandbox model:
+ OSR2
```

Any changes or additions you make to the sandbox model will have to rely on the definitions already made by the `OSR2`model.

### Overriding classes

Overriding existing classes is the most straightforward method to change the model's behaviors.

Overriding a class requires you to (a) know the exact name of the class you wish to override and (b) know exactly how the class should be defined.


We you override a class you are completely replacing the class definition hexroll uses from `OSR2`. Any missing attribute or incorrect specification of the class you define may cause issues with the generated sandbox.

For this reason, it is recommended to override only classes listed in this document, as they are considered safe for this purpose.

### Patching classes

Patching a class allows changing only a part of an already defined class. This is useful if we only want to change a single aspect of a complex parent class, later being used by an already defined `OSR2` class.

### Class extensions

Some classes allow plugging-in externally defined extensions.
This is the safest way to extend HEXROLL and the recommended method
if you want to just **add** content to existing entities.

### Defining new classes

Finally, in any of the previously mentioned setups, you can add any number of completely new classes you design and need for your specific scenario.



## Extensions
The following extension classes are ready for you to override
and easily add content to several key entities:

### Realm Extension

#### Class Specification

```
RealmExtension {
  Prescript! :: STRING
  Postscript! :: STRING
}
```

#### Injected Attributes
None.

#### Example

The realm deities scroll is adding a set of deities
to a realm, and specifying the deities list at the
end of the realm description.

From [https://scroll.hexroll.app/RTq5YUJS](https://scroll.hexroll.app/RTq5YUJS)

```
RealmExtension {
    [3..5 dieties] @ Deity
    Prescript! = ""
    Inscript! = ""
    Postscript! = <%
      {%for d in dieties%}
        {{d.Description}}
      {%endfor%}
    %>
}
```

Note how we use the extension to contain a new list of random deities and then list the deities in the `Postscript` attribute. This will make sure the deities are listed at the end of the Realm description page in the sandbox.



### Region Extension
#### Class Specification
```
RegionExtension {
  Prescript! :: STRING
  Inscript! :: STRING
  Postscript! :: STRING
}
```
#### Injected Attributes
None
#### Example
[TODO]

### Hex Extension
#### Class Specification
```
HexExtension {
  Prescript! :: STRING
  Inscript! :: STRING
  Postscript! :: STRING
}
```
#### Injected Attributes

`ExtensionTypeClass` - one of: 
`"ForestHexExtension"`,
`"MountainsHexExtension"`,
`"PlainsHexExtension"`,
`"DesertHexExtension"`,
`"JungleHexExtension"`,
`"SwampsHexExtension"`,
`"TundraHexExtension"`

Allows you to roll your own biome-based classes and use them in your extension:

```
HexExtension {
    MyBiomeSpecificEntity @ &ExtensionTypeClass
}
```

#### Example
TODO


### Character Extension
#### Class Specification
```
CharacterExtension {
  Inscript! :: STRING
  Postscript! :: STRING
}
```
#### Injected Attributes
`Name` - Character name entity

#### Example

In the following example, we use the extension to pick a random pre-generated deity for the character, and make sure it is listed as part of the character description by defining the `Postscript` attribute.

From [https://scroll.hexroll.app/RTq5YUJS](https://scroll.hexroll.app/RTq5YUJS)


```
CharacterExtension {
  Deity! % Deity # We use the %(pick) operator
  Prescript! = ""
  Inscript! = ""
  # The Postscript value will append the provided
  # template text to the character description.
  Postscript! = <% 
    <ul><li>
    Follower of {{Deity.FollowerDescription}}
    </li></ul>
  %>
}
```


### Dungeon Extension
#### Class Specification
```
DungeonExtension {
  Prescript! :: STRING
  Inscript! :: STRING
  Postscript! :: STRING
}
```
#### Injected Attributes
None
#### Example
[TODO]

## Names

Mods can help you replace the default name generators HEXROLL is using for Realms, Regions, Settlements, Dungeons and Characters, 

### Character Names

The `Name` class is used to randomize names for any generated character.

```
Name {
  Last! :: STRING
  First! :: STRING
  Full! :: STRING
}
```

#### Using Gender

Any time hexroll randomizes a name using the `Name` class, it injects a special attribute called `GenderNameClass`.
This attribute can have one of three values: `"NameFemale"`, `"NameFluid"` and `"NameMale"`.

You can use this attribute to roll a first name class like so:

```
NameFemale {
    Value @ [
        * Barbi
    ]
}

NameMale {
    Value @ [
        * Ken
    ]
}

NameFluid {
    ^ [
        * NameMale
        * NameFemale
    ]
}

Name {
  FirstNameByGender @ &GenderNameClass
  Last! = "Smith"
  First! = "{{FirstNameByGender.Value}}"
  Full! = "{{First}} {{Last}}"
}
```
#### Example

Take a look at the alternative names mod:
[https://scroll.hexroll.app/9sLC31em](https://scroll.hexroll.app/9sLC31em)


### Realm Names

You can override the `RealmName` class to override the default realm name generator.

#### Example

```
RealmName {
    Name @ [
        * Arnor
        * Rohan
        * Dúnedain
        * Elendil
        * Gondor
        * Isildur
        * Lonely Mountain
        * Morgoth
        * Sauron
    ]
    Title! ` "{{Name}}"
}
```


### Region Names

```
RegionName {
    Value! :: STRING
}
```

### Settlement Names
[TODO]

## Hex Features

To add new hex features you will need to:

1. Create a new sub-class of the `Feature` class
2. Override the default `Feature` class and include your new class in its sub-class list.

### Creating a new Feature class

```
WaterWell(Feature) {

	Name! ~ "Water Well"
	Description~ ~ <%
		A stone made water well
	%>
	
	Supplemental! ~ <%
		Anyone throwing a coin into the well
		will be...
	%>
}

Feature { ^ [
    *(x4) WaterWell

    *(x9) Bridge
    *(x9) Watchtower
    *(x9) DeadAdventurers
    *(x9) Wagons
    *(x9) AbandonedVillage
    *(x9) Altar
    *(x9) SacrificialSite
    *(x9) SignalingTower
    *(x9) DeadMonster
    *(x6) Graveyard
    *(x3) Arena
    *(x5) CaravanCamp
    *(x1) Artifact
    *(x1) Portal
    ]
}

```


### Example

* [https://scroll.hexroll.app/YSEXW3qX](https://scroll.hexroll.app/YSEXW3qX)



## Monsters

Defining a new monster class requires completing the following specification:

```
Monster {
  Title! :: STRING
  HitDice! :: INTEGER
  HitDiceRoll! :: STRING
  Alignment! :: STRING
  Intelligent! :: OPTIONAL(BOOL)
  Activity! :: OPTIONAL(TYPE(MonsterActivity))
  Sound! :: OPTIONAL(STRING)
  NumberAppearingRoaming! :: DICE
  NumberAppearingLair! :: DICE
  TreasureType! :: TYPE(TreasureType)
  Stats! :: STRING
}
```

Here's an example:

```
Azuga (Monster) {
  Title! ~ "Azuga"
  Alignment! = "Chaotic"
  HitDice! = 1
  HitDiceRoll! = "0d0+10"
  NumberAppearingRoaming! @ 1d8
  NumberAppearingLair! @ 2d6
  Stats! ~ <% F: 5 D: 7 %>
  TreasureType! @ TreasureTypeNone
  | Monster
}
```

Note that we must expand the parent class `Monster` at the end.

## Treasure

```
TreasureType {
  Empty! :: BOOL
  Details! :: STRING
}
```

[TODO]

## Encounters

Encounter classes are used throughout the generator to randomly roll a monster based on the biome or dungeon type.
Each encounter class lists a set of specific Monster subclasses to roll from.

### Dungeon Encounters

Dungeon encounter classes are organized by monster types and encounter tier. Encounter tiers are ranked from 1 to 4, 1 being lower HD monsters and 4 being the highest HD monsters per type.

Overriding any or all of the following class will change the set of monsters appearing in different places in the sandbox:

#### Caves
* `MonsterCavernsTier1`, `MonsterCavernsTier2`
#### Undead: 
* `MonsterUndeadTier1`, `MonsterUndeadTier2`, `MonsterUndeadTier3`, `MonsterUndeadTier4`
#### Aberrations:
* `MonstersAberrationTier1`, `MonstersAberrationTier2`, `MonstersAberrationTier3`, `MonstersAberrationTier4`
####  Dragons:
* `MonstersDragonsTier3`, `MonstersDragonsTier4`
####  Humanoids
* `MonstersHumanoidTier1`

####  Magical:
* `MonstersMagicalTier1`, `MonstersMagicalTier2`, `MonstersMagicalTier3`, `MonstersMagicalTier3`

#### Oozes
* `MonstersOozeTier1`, `MonstersOozeTier2`, `MonstersOozeTier3`

####  Vermins
* `MonstersVerminTier1`, `MonstersVerminTier2`, `MonstersVerminTier3`

####  Other
* `MonsterCultistsTier1`, `MonsterTempleAnimalsTier1`



### Wilderness Encounters

#### Feature Encounters:
* `DesertFeatureEncounter`, `ForestFeatureEncounter`, `MountainsFeatureEncounter`, `OceanFeatureEncounter`, `PlainsFeatureEncounter`, `SwampsFeatureEncounter`, `TundraFeatureEncounter`

#### Random Encounters

* `DesertRandomEncounter`, `ForestRandomEncounter`, `MountainsRandomEncounter`, `OceanRandomEncounter`, `PlainsRandomEncounter`, `SwampsRandomEncounter`, `TundraRandomEncounter`

### Trap Encounters

`PitTrapMonster`


### Examples

#### The built-in OSR Dungeons encounters:

* [/osr/encounters/dungeon.scroll](https://github.com/hexroll/hexroll-osr-scrolls/blob/master/osr/encounters/dungeon.scroll)

#### The built-in OSR Wilderness encounters:

* [/osr/encounters/wilderness.scroll](https://github.com/hexroll/hexroll-osr-scrolls/blob/master/osr/encounters/wilderness.scroll)

#### The Dragonbane example mod

* [https://scroll.hexroll.app/1yfeXmyK](https://scroll.hexroll.app/1yfeXmyK)


## Building a Mod

One thing to pay attention to, is that if you combine several scrolls
together for your mod, you might get one scroll override the other.

For example, if you have two scrolls defining the `CharacterExtension`
extension, only one of them will actually have an effect.

In such cases you may need to fuse both scrolls together using a new
custom made scroll.
