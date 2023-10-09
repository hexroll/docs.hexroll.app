# Editing the Maps

Before exploring how you can edit your sandbox map, it is important to note that Hexroll is not a hex map editor. Hex maps in Hexroll are an artifact derived from randomly generated sandbox content. This means that any map editing function Hexroll offers is first being applied on sandbox entities. The visual map is redrawn or refreshed only as a side-effect.

For example, if you choose to remove a complete generated realm, Hexroll will go ahead and remove each NPC, each treasure, each monster, hex and so on, before finally removing the actual visual representation of the realm.

## Unlocking the map

Maps are, by default, locked and immutable. To edit a map
you will first need to unlock it for editing. This is done
by clicking the lock icon on the bottom-left side of the map (TODO: Keyboard-shortcut).
If you want to edit a map, you will need to `unlock` it first.

![Map Unlock](/images/unlock.jpg)

Click the lock symbol at the bottom left side of the screen.

## New Realms

The `plus` button allows you to generate a new realm to randomly border an existing realm. When you click it, the realm generator window will appear, allowing you to tweak the new realm differently.

This button will open up the new realm generator and will append the new realm to any existing landmass.

Generating realms in other locations on the map, or generating other map features require's using the hex menu.

## The Hex Menu

The main map editing tool in Hexroll is the hex menu. This menu will show up if you click any hex while the map is unlocked for editing.


![menu](/images/menu.svg)

!!! tip
    Hovering over a hex tool button will reveal its tool tip.

### Rearrange Realm Hex Map

*
![rearrange button](/images/button_rearrange.png)
Rearrange realm regions and hexes - use it to reshape the map



### Rolling new features or land
![roll button](/images/button_roll.png)

When you choose to roll new features or land, you will be presented with the following options:

* 
![roll dungeon](/images/button_dungeon.png)
Roll a dungeon - this option will be enabled only if you select an empty land hex.

*  
![roll settlement](/images/button_settlement.png)
Roll a settlement - this option will be enabled only if you select an empty land hex.
 
*  
![roll river](/images/button_river.png)
Roll a river - this option will be enabled only if you select a mountain hex bordering a non-mountain, non-ocean hex.


*  
![roll realm](/images/button_realm.png)
Roll a realm - this option will be enabled only if you select once of the ocean hexes.

### Removing or resetting hex content
![broom button](/images/button_broom.png)

When you choose to remove or reset hex content, you will be presented with the following options:


*  
![remove river](/images/button_river.png)
Remove a River - clears the entire river across all hexes.

!!! warning
    Clearing a river that goes through settlements will alter the settlement
    map!
 
*  
![rebuild river](/images/button_trail.png)
Rebuild Trail - attempt to optimize any connected trails. This might help
removing redundant trails paths.

*  
![reroll hex](/images/button_feature.png)
Reroll an Empty Hex - clears any feature that is already in the hex and rolls a
new empty hex instead.

!!! warning
    Any settlements, NPCs and dungeons in the hex will be gone forever.


### Removing generated land
![trash button](/images/button_trash.png)
When you choose to remove generated land, together with any content (Settlements, Dungeons, NPCs or other content you have added manually!), you will be presented with the following options:

*  
![remove hex](/images/button_hex.png)
Remove a single Hex - together with everything in it

*  
![remove region](/images/button_region.png)
Remove the full Region - together with everything in it

*  
![remove realm](/images/button_realm.png)
Remove the entire Realm - together with everything in it

