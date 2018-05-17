

### Using story code to introduce custom Talents

Here's how I use story scripting to implement custom talents for a talent pack I created with another member of the D:OS 2 modding communitiy, getting around hardcoded limitations to open up new options for character builds. (Said pack has yet to be released. As of this writing, we are still squishing bugs.)

![Image](https://i.imgur.com/qgeJl1t.jpg)

Though the Divinity Engine doesn't offer any options for creating new talents, the modding community took early notice of a few hopeful quirks:

 * A long list of cut talents from D:OS 2 and leftover talents from D:OS 1 can be added to characters via scripting, but they aren't fully functional, and they do not show up in character sheets under normal conditions.
 * These deprecated talents can be succesfully attached to armor pieces, which will make them visible on the character sheet as long as the armor piece stays equipped.

Once Larian opened up D:OS 2's localization files to modders, it was possible to take a non-functioning deprecated talent, script new effects, change it's name and description, and create something indistinguishable from a brand new talent in every respect but one very essential one: No matter how cool your new perk was, it would remain hopelessly attached to a breastplate or a pair of trousers. All you had was an armor effect on a different character sheet.

There were however three underutilized armor slots that didn't show up on the character sheet, used for temporary effects granted by spells that the engine considered equipped items: the Wings slot, for Polymorph effects visible on the back; the Horns slot, for Polymorph effects shown on the head; and the Overhead slot, for the floating 'Inner Demon' visual effect. These slots weren't filled nearly as much as a players Weapon, Helmet, or Ring slots, but they were still necessary for a number of common skills, at least as they were originally coded.

If polymorph skills were decoupled from their armor effects entirely, it would open these slots up as nodes for custom talents, attached to players via invisible 'armor.' To preserve the visual transformation of Polymorph skills, you could use the Transform call to have them trigger the visual transformation of these talent-granting items, which would revert to invisibility at the end of the effect. Using this method you could implement new talents with the following limitations, which we considered pretty reasonable:

 * These talents would have to be selected from a dialogue menu, as we were still prevented from making them show up as selectable before they were granted.
 * Players could choose a maximum of three custom talents, limited by the number of non-gear 'armor' slots we had to work with.
 
![Image](https://i.imgur.com/miAt51k.jpg)
