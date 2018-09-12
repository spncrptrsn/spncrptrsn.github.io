### Scripting a player-driven quest with varied outcomes

For this quest, I started with this relatively simple premise, provided by Larian Studios as part of their scripting test. (Larian provided the level; I created the characters, scripted the quest and npc behaviors, and wrote the dialogue. My scripts can be found [here](https://github.com/spncrptrsn/spncrptrsn.github.io/tree/master/quest_scripts).)

>Richard the mayor of the human village of Glenrock has a letter that needs to be delivered to his mistress Sabrina who is drinking away in the tavern. In this letter he states his love for her and that he wants to leave his wife Jane to spend the rest of his life with Sabrina. Sabrina is at the tavern drinking because she can't stand Jackson her husband who works as a grocer at the local grocery store. Jackson has been suspecting Sabrina of cheating on him for quite a while now, he keeps track of her whereabouts in his diary. He can't say anything to Sabrina about her misbehavior because she knows about his dark life before he came to Glenrock. On many occasions she has threatened to tell Liam, the guard captain, Jackson's secrets in order to lock him away for good if he didn't stop being so jealous and suspicious of her.

My take on Glenrock (outlined in full in [this chart](https://www.lucidchart.com/documents/view/04bf2714-35b7-4376-a7ae-37b7e30ab1db/0#)) gave Richard an ulterior motive for presenting the player with this task. Richard and Sabrina's supposed affair is a ruse meant to trick the player into killing Liam, the captain of the guard. If the player follows their orders without picking up on this ulterior motive, Liam will be killed, and Richard and Sabrina will frame the player for it, turning the town guards hostile against them. 

[![Image](https://i.imgur.com/PShGSCj.jpg)](https://youtu.be/h333WE04yDA)

I planned this less as a 'bad ending' and more as an 'uninquisitive ending.' Inquisitive players can begin to unravel the criminal conspiracy afoot in the town by learning that the tarot card reader in the town square is effectively the gatekeeper to the local drug trade. Interact with her without this knowledge and she'll gladly read your fortune:

[![Image](https://i.imgur.com/u4f2gh5.jpg)](https://youtu.be/xiIFsEYd0Lk)

This basic reading is fully functional, powered by a dialog tree that randomly selects different 'cards.'

![Image](https://i.imgur.com/uBkqs1v.png)

If the player hears from the beggar or one of the mercenaries how to get a special reading from the tarot reader, they can use that to solve a puzzle in the tavern opening the hatch to the undertavern.

[![Image](https://i.imgur.com/3eKaEHo.jpg)](https://www.youtube.com/watch?v=tboJ14PlJnw)

This puzzle was implemented as a lore-friendly version of the lever puzzle aspect of Larian Studio's scripting test.

>There’re 3 levers/buttons somewhere in Glenrock. Pressing them in a specific order creates something positive for the player (e.g.: a secret door unlocks, a useful item appears, ...). If the three levers are pulled in the wrong order, something negative happens (e.g.: players get teleported, receive damage, ...). This should only happen after a 3rd lever is pulled.

The Osiris code I wrote to power this puzzle is as follows. Each block of code below is what's referred to as a 'Rule,' consisting of a trigger conditions (IF), optional extra conditions (AND), and actions (THEN). Every action ends in a semi-colon. Actions beginning with DB_ add new database 'Facts' that can be checked as extra conditions. Removing database facts is done by prepending them with the NOT operator. Queries (QRY) are used to implement OR conditions, and are considered to have succeeded if all conditions of at least one definition of this query (attempted an descending order) was executed.

```
IF 
CharacterItemEvent((CHARACTERGUID)_Thrower,(ITEMGUID)_Target,"LA_DartboardHit")
AND
QRY_LA_GetOrder(_Thrower,_Target)
AND
DB_LA_ThreeHit(ITEMGUID_LA_TarotCard_009_a9a631fb-39d5-4834-a252-4410df0bc7b7,ITEMGUID_LA_TarotCard_002_65c9b81c-747f-4bf6-8648-bbfced100c51,ITEMGUID_LA_TarotCard_008_166ae2d9-acc2-4c7f-9b17-d42cf6646c80)
THEN
ObjectSetFlag(_Thrower,"LA_CorrectOrder");

QRY
QRY_LA_GetOrder((CHARACTERGUID)_Thrower,(ITEMGUID)_Third)
AND
DB_LA_TwoHit((ITEMGUID)_First,(ITEMGUID)_Second)
THEN
DB_LA_ThreeHit(_First,_Second,_Third);
ProcObjectTimer(_Thrower,"LA_DwarfConvo",1500);
 
QRY
QRY_LA_GetOrder((CHARACTERGUID)_Thrower,(ITEMGUID)_Second)
AND
DB_LA_OneHit((ITEMGUID)_First)
AND 
NOT DB_LA_TwoHit(_,_)
THEN
DB_LA_TwoHit(_First,_Second);

QRY
QRY_LA_GetOrder((CHARACTERGUID)_Thrower,(ITEMGUID)_First)
AND
NOT DB_LA_OneHit(_)
THEN
DB_LA_OneHit(_First);

PROC
ProcObjectTimerFinished((CHARACTERGUID)_Thrower,"LA_DwarfConvo")
AND
DB_LA_ThreeHit(_First,_Second,_Third)
THEN
Proc_StartDialog(0,"LA_DartResult",CHARACTERGUID_LA_Dartmaster_8856222a-d080-4020-89ba-2f119471e0d4,_Thrower);
NOT DB_LA_OneHit((ITEMGUID)_First);
NOT DB_LA_TwoHit(_First,(ITEMGUID)_Second);
NOT DB_LA_ThreeHit(_First,_Second,(ITEMGUID)_Third);

IF
GlobalFlagSet("LA_GLO_HatchAppear")
THEN
SetOnStage(ITEMGUID_LA_InteriorTavernHatch_33529c6d-02f9-48c6-9d81-ca0df6caa9d8,1);
PlayEffect(ITEMGUID_LA_InteriorTavernHatch_33529c6d-02f9-48c6-9d81-ca0df6caa9d8,"RS3_FX_Skills_Earth_Cast_Ground_Earth_Root_01");
```
