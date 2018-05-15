### Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, sacrificing some of the excitement that can come from uncertainty: the elation of succeeding on a 20% attempt to petrify some monstrosity bearing down on you, the affront of getting hit by a hail mary 5% charm arrow.

Armor-Based Saving Throws is my attempt to inject a little bit of D&D-style debuffing and XCOM-style risk management gameplay into D:OS 2. Implementing a fully stat-based saving throw system would have thrown out what was great about 'health bar' armor: how it allows the player to point to a unit and know exactly how vulnerable it is; how it makes the player work for debuffs by softening up a target first. ABST retains these strengths, but turns a unit's percentage of armor remaining into their _chance_ to resist physical or magic debuffs. 

This is accomplished with scripting, by altering instances of the FetchCharacterApplyStatusData event, which filters incoming status effects before they're resolved on targets. Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), nullifying the status if the target's score is greater.

![Image](https://i.imgur.com/LREhPza.jpg)

That ABST function also generates a combat log entry showing the resolution of the formula, recreating the style and color-coordination of vanilla combat log entries. These notes are there for presentation and clarity's sake, but they're also of particular importance in this context. RNG already has a tendancy to leave players feeling disempowered. Giving them 'perfect information' ensures that both sides are playing fairly.

Another important factor with RNG is to give players have opportunities to rig the numbers in their favor; to say yes, god does indeed laugh when you make plans, but with enough cunning you can still pull this off. Initially, this was done by attaching minor innate bonuses to a few stats and abilities—Wits, Constitution, Perseverance, and Retribution—chosen to encourage build diversity, because they were considered less-than-optimal picks in the base game. Bless and Curse were altered to also grant advantage or disadvantage on saving throws (roll twice and pick the highest or lowest, respectively) reinforcing the centrality of these story-relevant statuses rather than inventing new combat dynamics out of whole cloth.

When Larian enabled the modding of D:OS 2's localization files, this opened the floodgates for ABST to allow players to tip the scales even more. By scripting in extra events checking for player talents, and altering talent descriptions to convey those new effects, you could effectively augment these hard-coded perks, boosting the less powerful ones by tying them into the saving throw formula.

![Image](https://i.imgur.com/0UVQHHi.jpg)
![Image](https://i.imgur.com/xn2iCVZ.jpg)

A player who fails a flat 50% roll to set Chicken Form may come away resenting what RNG stole from them. A player who can emerge from Smoke (advantage from Guerrilla), pop Adrenaline, Backlash into position (+5 from Sword Dancer), use Flurry (+5 from Sword Dancer), and finish with an enhanced Chicken Touch attempt (an extra roll from Guerilla, +10 from Sword Dancer) is a player who used their cunning to skew the numbers in their favor, and might relishes that succesful Chickening all the more for it.

![Image](https://i.imgur.com/5u0llvb.jpg)
