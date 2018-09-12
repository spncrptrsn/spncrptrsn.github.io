### Design decisions behind Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, sacrificing some of the excitement that can come from uncertainty: the elation of succeeding on a last-ditch 20% attempt to petrify some monstrosity bearing down on you, the affront of having your next turn stolen by a Hail Mary charm arrow with a 5% chance to succeed.

In D:OS 2, physical and magic defense aren't embodied in abstract armor class, saving throw, or damage reduction values. Each character has two extra health bars representing their phsyical and magic armor. Physical and magic damage degrade each of these, respectively, only biting into a character's health pool when their armor level is 0 for that damage type. Armor also works to block debuffs. Any amount of armor will block a status effect. If a character with 75/100 magic armor gets attacked by a 74-damage Electric Discharge, their remaining 1 magic armor will completely block the incoming Shocked status that is also part of that skill.

![Image](https://i.imgur.com/9YKeMwi.jpg)

[Armor-Based Saving Throws](https://steamcommunity.com/sharedfiles/filedetails/?id=1157299447) is my attempt to inject a little bit of D&D-style debuffing and XCOM-style risk management gameplay into D:OS 2. Implementing a fully stat-based saving throw system would have thrown out what was great about D:OS 2's 'health bar' armor: how it allows the player to point to a unit and know exactly how vulnerable it is; how it makes the player work for debuffs by softening up a target first. ABST retains these strengths, but turns a unit's percentage of armor remaining into their _chance_ to resist physical or magic debuffs. 

This is accomplished with scripting, by creating a rule that triggers every time an incoming debuff is blocked by armor. An invisible 'dummy status' is then applied to that character.

```
IF
CharacterStatusAttempt((CHARACTERGUID)_Char,(STRING)_Status,(CHARACTERGUID)_Caster)
AND
ObjectGetFlag(_Char,"ABST_StatusBypass",0)
AND
DB_ABST_PhysProcStatus(_Attempt,_Status,_)
AND
CharacterGetArmorPercentage(_Char,(REAL)_PhysArmor)
AND
_PhysArmor > 0
AND
NOT QRY_ABST_SurfaceConcern(_Char,_Status)
//AND
//QRY_ABST_SurfaceBoost(_Char,_Caster,_Status)
THEN
SetVarObject(_Char,"ABST_Caster",_Caster);
ApplyStatus(_Char,(STRING)_Attempt,6.0,0,_Caster);
```

This 'dummy status' (in this case, OVR_CHICKEN) is then caught by a custom FetchCharacterApplyStatusData event, which filters incoming status effects before they are resolved on targets. 

```
EVENT ABSTCharacterSetChicken
VARS
	CHARACTER:_Character
	LIST<STATUS>:_RemoveList
	STATUS:_Result
	STRING:_String
	INT:_Saved
ON
	FetchCharacterApplyStatusData(_Character, OVR_CHICKEN)
ACTIONS
	IF "c1|c2"
		CharacterHasStatus(_Character,ABST_AEGIS)
		CharacterHasStatus(_Character,ABST_MORNING)
	THEN
		SetFlag(_Character,"ABST_MorningFlag")
	ENDIF
	Print(%StatusString1,"<font color='#f7ba14'>Chicken Form</font>")
	Print(%StatusString2," was turned into a <font color='#f7ba14'>Chicken</font>")		
	Set(_Result, OVR_CHICKEN)
	ListClear(_RemoveList)
	Set(%Char,_Character)
	CallFunction("PhysArmorTally")
	Set(_Saved,%Saved)
	IF "c1"
		IsEqual(_Saved,1)
	THEN
		Set(_Result,null)
	ENDIF
	ClearFlag(_Character,"ABST_MorningFlag")
	RETURN(_RemoveList,_Result,1)
  ```
  
Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), forcing the original debuff through the target's remaining armor if they fail their save. ([Full scripts here](https://github.com/spncrptrsn/spncrptrsn.github.io/tree/master/abst_scripts).)

![Image](https://i.imgur.com/LREhPza.jpg)

That ABST function also generates a combat log entry showing the resolution of the formula, recreating the style and color-coordination of vanilla combat log entries. These notes are there for presentation and clarity's sake, but they're also of particular importance in this context. RNG already has a tendancy to leave players feeling disempowered. Giving them 'perfect information' ensures that both sides are playing fairly.

Another important factor with RNG is to give players opportunities to rig the numbers in their favor; to say yes, god does indeed laugh when you make plans, but with enough cunning you can still pull this off. Initially, this was done by attaching minor innate bonuses to a few stats and abilities—Wits, Constitution, Perseverance, and Retribution—chosen to encourage build diversity, because they were considered less-than-optimal picks in the base game. Bless and Curse were altered to also grant advantage or disadvantage on saving throws (roll twice and pick the highest or lowest, respectively) reinforcing the centrality of these story-relevant statuses rather than inventing new combat dynamics out of whole cloth.

When Larian enabled the modding of D:OS 2's localization files, this opened the floodgates for ABST to allow players to tip the scales even more. By scripting in extra events checking for player talents, and altering talent descriptions to convey those new effects, you could effectively augment these hard-coded perks, boosting the less powerful ones by [tying them into the saving throw formula](https://steamcommunity.com/workshop/filedetails/discussion/1505329732/2590022385656340131/). This is great from a character creation standpoint, because harder build choices make for happier players.

![Image](https://i.imgur.com/0UVQHHi.jpg)
![Image](https://i.imgur.com/xn2iCVZ.jpg)

A player who fails a flat 50% roll to polymorph an enemy into a chicken may come away resenting what RNG stole from them. A player who can emerge from Smoke (gaining advantage from Guerrilla), Backlash into position (gaining +5 from Sword Dancer), use Flurry (another +5 from Sword Dancer), and finish with a juiced-up chicken polymorph attempt (with advantage and a +10 bonus) is a player who was empowered to rig the numbers in their favor, who just might relish that succesful chickening all the more for it.

![Image](https://i.imgur.com/5u0llvb.jpg)
