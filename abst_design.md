### Design decisions behind Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, sacrificing some of the excitement that can come from uncertainty: the elation of succeeding on a last-ditch 20% attempt to petrify some monstrosity bearing down on you, the affront of having your next turn stolen by a Hail Mary charm arrow with a 5% chance to succeed.

[Armor-Based Saving Throws](https://steamcommunity.com/sharedfiles/filedetails/?id=1157299447) is my attempt to inject a little bit of D&D-style debuffing and XCOM-style risk management gameplay into D:OS 2. Implementing a fully stat-based saving throw system would have thrown out what was great about 'health bar' armor: how it allows the player to point to a unit and know exactly how vulnerable it is; how it makes the player work for debuffs by softening up a target first. ABST retains these strengths, but turns a unit's percentage of armor remaining into their _chance_ to resist physical or magic debuffs. 

This is accomplished with scripting, by altering instances of the FetchCharacterApplyStatusData event, which filters incoming status effects before they are resolved on targets. 

```
EVENT ABSTCharacterSetChilled
VARS
	CHARACTER:_Character
	LIST<STATUS>:_RemoveList
	STATUS:_Result
	INT:_Turns
ON
	FetchCharacterApplyStatusData(_Character, OVR_CHILLED)
ACTIONS
	ListClear(_RemoveList)
	IF "c1"
		CharacterHasStatus(_Character, BURNING)
	THEN
		ListAdd(_RemoveList, BURNING)
		Set(_Result, WARM)
	ELIF "c1|c2|c3"
		CharacterHasStatus(_Character, FROZEN)
		CharacterHasStatus(_Character, NECROFIRE)
		CharacterHasStatus(_Character, HOLY_FIRE)
	THEN
		Set(_Result,null)
	ELIF "c1"
		CharacterHasStatus(_Character, WARM)
	THEN
		ListAdd(_RemoveList, WARM)
		Set(_Result,null)
		ELIF "c1|c2"
			CharacterHasStatus(_Character, CHILLED)
			CharacterHasStatus(_Character, OVR_CHILLED)
		THEN
		Set(_Result, OVR_FROZEN)
		Set(_Turns,1)
	ELSE
		Print(%StatusString,"<font color='#cfecff'>Chilled</font>")
		Print(%StatusString2," was [1]", %StatusString)
		Set(_Result,OVR_CHILLED)
		Set(_Turns,null)
		Set(%Char,_Character)
		CallFunction("MagicArmorTally")
		IF "c1"
			IsEqual(%Saved,1)
		THEN
			Set(_Result,DEF_CHILLED)
		ELIF "c1"
			CharacterHasStatus(_Character, WET)
		THEN
			Set(_Result, OVR_FROZEN)
			Set(_Turns,1)
		ENDIF
	ENDIF
	Set(%Surface,0)
	Set(%DuckFlag,0)
	RETURN(_RemoveList,_Result,null)
  ```
  
Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), nullifying the status if the target's score is greater.

![Image](https://i.imgur.com/LREhPza.jpg)

That ABST function also generates a combat log entry showing the resolution of the formula, recreating the style and color-coordination of vanilla combat log entries. These notes are there for presentation and clarity's sake, but they're also of particular importance in this context. RNG already has a tendancy to leave players feeling disempowered. Giving them 'perfect information' ensures that both sides are playing fairly.

Another important factor with RNG is to give players opportunities to rig the numbers in their favor; to say yes, god does indeed laugh when you make plans, but with enough cunning you can still pull this off. Initially, this was done by attaching minor innate bonuses to a few stats and abilities—Wits, Constitution, Perseverance, and Retribution—chosen to encourage build diversity, because they were considered less-than-optimal picks in the base game. Bless and Curse were altered to also grant advantage or disadvantage on saving throws (roll twice and pick the highest or lowest, respectively) reinforcing the centrality of these story-relevant statuses rather than inventing new combat dynamics out of whole cloth.

When Larian enabled the modding of D:OS 2's localization files, this opened the floodgates for ABST to allow players to tip the scales even more. By scripting in extra events checking for player talents, and altering talent descriptions to convey those new effects, you could effectively augment these hard-coded perks, boosting the less powerful ones by tying them into the saving throw formula. This is great from a character creation standpoint, because harder build choices make for happier players.

![Image](https://i.imgur.com/0UVQHHi.jpg)
![Image](https://i.imgur.com/xn2iCVZ.jpg)

A player who fails a flat 50% roll to set Chicken Form may come away resenting what RNG stole from them. A player who can emerge from Smoke (gaining advantage from Guerrilla), trigger Adrenaline, Backlash into position (gaining +5 from Sword Dancer), use Flurry (another +5 from Sword Dancer), and finish with a juiced-up Chicken Touch attempt (with advantage and a +10 bonus) is a player who was empowered to rig the numbers in their favor, who just might relish that succesful chickening all the more for it.

![Image](https://i.imgur.com/5u0llvb.jpg)
