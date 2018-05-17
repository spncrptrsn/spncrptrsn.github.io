

### Using story scripting to introduce the first custom talents to Divinity: Original Sin 2

Here's how I use story scripting to implement custom talents for my upcoming talent pack, getting around hardcoded limitations that modders have lived with since D:OS1. (As of this writing, the mod is being playtested by a few other members of the community before release next week with an initial batch of 12 new talents.)

![Image](https://i.imgur.com/qgeJl1t.jpg)

Though the Divinity Engine doesn't offer any options for creating new talents, modders took early notice of a few hopeful quirks:

 * A long list of cut talents from D:OS 2 and leftover talents from D:OS 1 can be added to characters via scripting, but they aren't fully functional, and do not show up in character sheets under normal conditions.
 * These deprecated talents can be succesfully attached to armor pieces, which _will_ make them visible on the character sheet as long as the armor piece stays equipped.

Once Larian opened up D:OS 2's localization files to modders, it was possible to take a non-functioning deprecated talent, script new effects, change it's name and description, and create something indistinguishable from a brand new talent in every respect but one very essential one: No matter how cool your new perk was, it would remain hopelessly attached to a breastplate or a pair of trousers. Essentially all you had was an armor effect that appeared on a different character sheet.

However, there were three underutilized armor slots that did not show up on the character sheet, used for temporary effects granted by spells that the engine considered equipped items: the Wings slot, for Polymorph effects visible on the back; the Horns slot, for Polymorph effects shown on the head; and the Overhead slot, for the floating 'Inner Demon' visual effect. Though these slots weren't filled nearly as much as a players Weapon, Helmet, or Ring slots—making them potential platforms for adding armor-attached mods without taking up one of the players _real_ armor slots—they were still necessary for a number of common skills, at least as they were originally coded.

But if polymorph skills were decoupled from their armor effects entirely, it would open these slots up as nodes for custom talents, attached to players via invisible 'armor.' To preserve the visual effects of Polymorph skills, one could use the Transform call to have them trigger the superficial transformation of these hidden talent-granting items, which would revert to invisibility at the end of the effect. I used this method to implement new talents with the following limitations:

 * Because these talents were still prevented from being displayed on the character's list of greyed-out potential talents, they would have to be selected from a dialogue menu.
 * Players can choose a maximum of three custom talents, limited by the number of non-gear 'armor' slots I had to work with.
 
![Image](https://i.imgur.com/miAt51k.jpg)

The player's unspent talent points are checked when the dialogue triggers, and one is removed upon selection. Talent requirements are also taken into account with this method, marked by a number of temporary flags, like one would use for any quest dialogue. 
