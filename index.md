### Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, but that simplification did away with some of the excitement that can come from uncertainty: the elation of making a low-chance roll to save a party member, the bitter sting of getting hit by someone's hail mary 5%-to-hit spell.

Armor-Based Saving Throws is my attempt to inject a little bit of D&D-style debuffing amd XCOM-style risk-taking/risk-mitigation into D:OS 2. Early-access forum posts proposed classic stat-based saving throws, but that would ditch what was great about the armor system--the ease with which you could point at a unit and know how close you were to pulling off a disable (without having to right-click, hit 'examine' and tally its Willpower score). I hoped a hybrid approach could provide the best of both worlds. 

This is accomplished with the script event FetchCharacterApplyStatusData, which the game uses to filter statuses launched by casters before they're resolved on targets, replacing them as needed when a launched status interacts with a target status to create a different result (i.e. Shocked upgrading to Stunned when applied to a Wet character).
```
EVENT CharacterSetShocked
VARS
	CHARACTER:_Character
	LIST<STATUS>:_RemoveList
	STATUS:_Result
	INT:_Turns
ON 
	FetchCharacterApplyStatusData(_Character, SHOCKED)
ACTIONS
	Set(_Result,SHOCKED)
	Set(_Turns,null)
	ListClear(_RemoveList)
	IF "c1"
		CharacterHasStatus(_Character, MAGIC_SHELL)
	THEN
		ListAdd(_RemoveList, MAGIC_SHELL)	
		Set(_Result,null)
	ELIF "c1"
		CharacterHasStatus(_Character, STUNNED)
	THEN
		ListAdd(_RemoveList, SHOCKED)	
		Set(_Result,null)
	ELIF "c1|c2"
		CharacterHasStatus(_Character, SHOCKED)
		CharacterHasStatus(_Character, WET)
	THEN
		ListAdd(_RemoveList, SHOCKED)	
		Set(_Result,STUNNED)
		Set(_Turns,1)
	ENDIF
	ListAdd(_RemoveList, INVISIBLE)
	ListAdd(_RemoveList, SLEEPING)
	RETURN(_RemoveList,_Result,_Turns)
```
Here's what it looks like:
```
EVENT ABSTCharacterSetShocked
VARS
	CHARACTER:_Character
	LIST<STATUS>:_RemoveList
	STATUS:_Result
	INT:_Turns
ON
	FetchCharacterApplyStatusData(_Character, OVR_SHOCKED)
ACTIONS
	ListClear(_RemoveList)
	IF "c1|c2"
		CharacterHasStatus(_Character, STUNNED)
		CharacterHasStatus(_Character, PETRIFIED)
	THEN
		Set(_Result,null)
	ELIF "c1|c2"
		CharacterHasStatus(_Character, SHOCKED)
		CharacterHasStatus(_Character,OVR_SHOCKED)
	THEN
		Set(_Result,OVR_STUNNED)
		Set(_Turns,1)
	ELSE
		Print(%StatusString,"<font color='#7d71d9'>Shocked</font>")
		Print(%StatusString2," was [1]", %StatusString)
		Set(_Result,OVR_SHOCKED)
		Set(_Turns,null)
		Set(%Char,_Character)
		IF "c1"
			CharacterHasStatus(_Character, MAGIC_SHELL)
		THEN
			Set(%StatusString2,"'s <font color='#4197e2'>Magic Shell</font> was purged")
		ENDIF
		CallFunction("MagicArmorTally")
		IF "c1"
			IsEqual(%Saved,1)
		THEN
			Set(_Result,DEF_SHOCKED)
		ELIF "c1"
			CharacterHasStatus(_Character, MAGIC_SHELL)
		THEN
			ListAdd(_RemoveList, MAGIC_SHELL)
			Set(_Result,ABST_LOG)
		ELIF "c1"
			CharacterHasStatus(_Character, WET)
		THEN
			Set(_Result,OVR_STUNNED)
			Set(_Turns,1)
		ENDIF
	ENDIF
	Set(%Surface,0)
	Set(%DuckFlag,0)
	RETURN(_RemoveList,_Result,null)
```
set by skills with 'trigger' statuses. Unlike, say, Chicken Form, which is fully blocked by physical armor in the base game, and ha

![Image](http://i.jeuxactus.com/datas/jeux/x/c/xcom-2/xl/xcom-2-56b1196908306.jpg)

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```
