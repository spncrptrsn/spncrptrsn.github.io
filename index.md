### Armor-Based Saving Throws

Divinity: Original Sin 2's armor system took the guess-work out of setting status effects, but that simplification did away with some of the excitement that can come from uncertainty: the elation of making a low-chance roll to save a party member, the bitter sting of getting hit by someone's hail mary 5%-to-hit spell.

Armor-Based Saving Throws is my attempt to inject a little bit of D&D-style debuffing amd XCOM-style risk-taking/risk-mitigation into D:OS 2. Early-access forum posts proposed classic stat-based saving throws, but that would ditch what was great about the armor system--the ease with which you could point at a unit and know how close you were to pulling off a disable (without having to right-click, hit 'examine' and tally its Willpower score). I hoped a hybrid approach could provide the best of both worlds. 

ABST injects uncertainty into the armor system by turning a character's percentage of armor remaining into their chance to resist physical or magic debuffs. This is accomplished with scripting, by altering instances of the event FetchCharacterApplyStatusData, which filters incoming status effects before they're resolved on targets. Each time this event fires, it now calls a custom function that 'rolls' a d100 (plus caster bonuses) against a target's armor percentage (plus target bonuses), nullifying the status if the target's score is greater, and generating a combat log entry detailing the resolution of the formula.




   
set by skills with 'trigger' statuses. Unlike, say, Chicken Form, which is fully blocked by physical armor in the base game, and ha

![Image](https://i.imgur.com/LREhPza.jpg)

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
