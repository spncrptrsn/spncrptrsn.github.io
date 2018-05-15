### Design decisions behind Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, sacrificing some of the excitement that can come from uncertainty: the elation of succeeding on a last-ditch 20% attempt to petrify some monstrosity bearing down on you, the affront of having your next turn stolen by a Hail Mary 5% charm arrow.

Armor-Based Saving Throws is my attempt to inject a little bit of D&D-style debuffing and XCOM-style risk management gameplay into D:OS 2. Implementing a fully stat-based saving throw system would have thrown out what was great about 'health bar' armor: how it allows the player to point to a unit and know exactly how vulnerable it is; how it makes the player work for debuffs by softening up a target first. ABST retains these strengths, but turns a unit's percentage of armor remaining into their _chance_ to resist physical or magic debuffs. 

This is accomplished with scripting, by altering instances of the FetchCharacterApplyStatusData event, which filters incoming status effects before they're resolved on targets. Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), nullifying the status if the target's score is greater.

![Image](https://i.imgur.com/LREhPza.jpg)

That ABST function also generates a combat log entry showing the resolution of the formula, recreating the style and color-coordination of vanilla combat log entries. These notes are there for presentation and clarity's sake, but they're also of particular importance in this context. RNG already has a tendancy to leave players feeling disempowered. Giving them 'perfect information' ensures that both sides are playing fairly.

Another important factor with RNG is to give players opportunities to rig the numbers in their favor; to say yes, god does indeed laugh when you make plans, but with enough cunning you can still pull this off. Initially, this was done by attaching minor innate bonuses to a few stats and abilities—Wits, Constitution, Perseverance, and Retribution—chosen to encourage build diversity, because they were considered less-than-optimal picks in the base game. Bless and Curse were altered to also grant advantage or disadvantage on saving throws (roll twice and pick the highest or lowest, respectively) reinforcing the centrality of these story-relevant statuses rather than inventing new combat dynamics out of whole cloth.

When Larian enabled the modding of D:OS 2's localization files, this opened the floodgates for ABST to allow players to tip the scales even more. By scripting in extra events checking for player talents, and altering talent descriptions to convey those new effects, you could effectively augment these hard-coded perks, boosting the less powerful ones by tying them into the saving throw formula. This is great from a character creation standpoint, because harder build choices make for happier players.

![Image](https://i.imgur.com/0UVQHHi.jpg)
![Image](https://i.imgur.com/xn2iCVZ.jpg)

A player who fails a flat 50% roll to set Chicken Form may come away resenting what RNG stole from them. A player who can emerge from Smoke (gaining advantage from Guerrilla), trigger Adrenaline, Backlash into position (gaining +5 from Sword Dancer), use Flurry (another +5 from Sword Dancer), and finish with a juiced-up Chicken Touch attempt (with advantage and a +10 bonus) is a player who was empowered to rig the numbers in their favor, who just might relish that succesful chickening all the more for it.

![Image](https://i.imgur.com/5u0llvb.jpg)

### Using story code and adaptive repurposing to introduce new Talents

Here's how I use story scripting to implement custom talents for a talent pack I created with another member of the D:OS 2 modding communitiy, getting around hardcoded limitations to open up new character build options. (Said pack has yet to be released. As of this writing, we are still squishing bugs.)

Though the Divinity Engine doesn't offer any options for creating new talents, the modding community took early notice of a few hopeful quirks:
 * A long list of cut talents from D:OS 2 and leftover talents from D:OS 1 can be added to characters via scripting, but many of them aren't fully functional, and cannot be viewed by players in their character sheets under normal conditions.
 * However, these deprecated talents can be succesfully attached to armor pieces, which will make them visible on the character sheet as long as the armor piece stays equipped.

Once Larian opened up D:OS 2's localization files to modders, it was possible to take a non-functioning deprecated talent, script new effects, change it's name and description, and create something indistinguishable from a brand new talent. Still, no matter how cool your new perk was, it would remain hopelessly attached to a breastplate or a pair of trousers; hardly something you could expect a player to base a character build that would carry them through the campaign around.

There were however three underutilized armor slots that didn't show up on the character sheet, used for temporary effects granted by spells that the engine considered equipped items: the Wings slot, for Polymorph effects visible on the back; the Horns slot, for Polymorph effects visible on the head; and the Overhead slot, for the floating 'Inner Demon' visual effect. These slots weren't filled nearly as much as a players Weapon, Helmet, or Ring slots, but they were still necessary for a number of common skills, at least as they were originally coded.

I turned this problem over in my head before considering the Transform call, which could be used to change the appearance of equipped items. Suppose that polymorph skills were decoupled from their armor effects entirely, and instead just prompted the transformation of items that the character already equipped? If every player was automatically equipped with three blank placeholder items, these could be transformed (stats only) to award repurposed deptracted talents (to a maxmium of three, bound by the number of slots), and later temporarily transformed (visuals only) when polymorph skills are used.


 
