### Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, but that simplification did away with some of the excitement that can come from uncertainty: the elation of making a low-chance roll to save a party member, the bitter sting of getting hit by someone's hail mary 5%-to-hit spell.

Armor-Based Saving Throws is my attempt to inject a little bit of D&D-style debuffing amd XCOM-style risk-taking/risk-mitigation into D:OS 2. Early-access forum posts proposed classic stat-based saving throws, but that would ditch what was great about the armor system--the ease with which you could point at a unit and know how close you were to pulling off a disable (without having to right-click, hit 'examine' and tally its Willpower score). I hoped a hybrid approach could provide the best of both worlds. 

ABST injects uncertainty into the armor system by turning a character's percentage of armor remaining into their chance to resist physical or magic debuffs. This is accomplished with scripting, by altering instances of the event FetchCharacterApplyStatusData, which filters incoming status effects before they're resolved on targets. Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), nullifying the status if the target's score is greater.

![Image](https://i.imgur.com/LREhPza.jpg)

Finally, a combat log entry is generated showing the resolution of the formula alongside vanilla combat log entries. This is there for presentation and clarity's sake, but it's of particular importance with this mechanic. RNG already has a tendancy to leave players feeling disempowered. Giving them 'perfect information' ensures that both sides are playing fairly.

Another important factor with RNG is to ensure that players have opportunities to bend the numbers in their favor; to say yes, god laughs when you make plans, but with enough cunning you can still pull this off. Initially, this was done with minor innate bonuses from stats and abilities; Wits, Constitution, and Perseverance would contribute to your saving throws, while Retribution would contribute to your status attempts. These stats were chosen to encourage build diversity, because they were usually less-than-optimal picks in the base game. Bless and Curse were later altered to grant advantage or disadvantage on saving throws (roll twice and pick the highest or lowest, respectively), taking a dynamic that already existed in the base game (two diametrically opposed super-statuses) and building upon it.

When Larian opened up D:OS 2's localization files to modders this opened the floodgates to allow players even more RNG manipulation. By scripting in extra events based on players talents, and altering talent descriptions, you could effectively alter these hard-coded perks to burnish less-than-stellar Talents by tying them into the saving throw formula.

![Image](https://i.imgur.com/7rhxZ48.jpg)
![Image](https://i.imgur.com/0UVQHHi.jpg)
![Image](https://i.imgur.com/xn2iCVZ.jpg)

A player who can emerge from smoke, pop Adrenaline, make two basick attacks and finish with a Chicken Touch that gets +10 to apply (Sword Dancer) and an extra roll (Guerrilla) is a player who really relishes that succesful Chickening.
