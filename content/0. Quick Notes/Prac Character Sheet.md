---
currentHP: 51
maxHP: 61
tempHP: 0
kiPoints: 6
maxKiPoints: 6
strStat: 3
dexStat: 3
conStat: 3
intStat: -1
wisStat: 3
chaStat: 0
hitDice: 3
maxHitDice: 5
hitDieSize: 8
---
> [!infobox]
> ```dataviewjs
> const keys = {
>   hp: "currentHP",
>   max: "maxHP",
>   temp: "tempHP"
> }
> 
> async function applyDamage(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.hp in fm)) fm[keys.hp] = 0
>     if (!(keys.temp in fm)) fm[keys.temp] = 0
> 
>     let temp = fm[keys.temp]
>     if (temp >= amount) {
>       fm[keys.temp] -= amount
>     } else {
>       fm[keys.temp] = 0
>       fm[keys.hp] = Math.max(0, fm[keys.hp] - (amount - temp))
>     }
>   })
> }
> 
> async function applyHealing(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.hp in fm)) fm[keys.hp] = 0
>     if (!(keys.max in fm)) fm[keys.max] = 0
>     fm[keys.hp] = Math.min(fm[keys.hp] + amount, fm[keys.max])
>   })
> }
> 
> async function addTempHP(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.temp in fm)) fm[keys.temp] = 0
>     fm[keys.temp] += amount
>  // Temp HP never stacks, only replaces if higher
>   })
> }
> 
> async function fullHeal(file) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!("maxHP" in fm)) fm["maxHP"] = 0
>     fm["currentHP"] = fm["maxHP"]
>   })
> }
> 
> 
> const current = dv.current()
> const file = app.vault.getAbstractFileByPath(current.file.path)
> 
> const healBtn = dv.el('button', 'Heal')
> healBtn.onclick = () => applyHealing(file, 1)
> 
> const dmgBtn = dv.el('button', 'Damage')
> dmgBtn.onclick = () => applyDamage(file, 1)
>
> const fullHealBtn = dv.el('button', 'Full Heal')
> fullHealBtn.onclick = () => fullHeal(file)
> 
> 
> const tempBtn = dv.el('button', 'Grant Temp HP')
> tempBtn.onclick = () => addTempHP(file, 1)
> 
> dv.paragraph(`<h1> HP: ${current.currentHP ?? 0} / ${current.maxHP ?? 0} | Temp HP: ${current.tempHP ?? 0} </h1>`)
> dv.span(healBtn); dv.span(dmgBtn); dv.span(fullHealBtn); dv.span(tempBtn)
>
> ```
> -----
> <p style="font-size: 18px;">Armour Class | 16</p>
> <p style="font-size: 18px;">Initiative | +3</p>
> <p style="font-size: 18px;">Proficiency Bonus | +3</p>
> <p style="font-size: 18px;">Speed | 70ft</p>
> 
> -----
> ```dataviewjs
> const keys = {
>   ki: "kiPoints",
>   max: "maxKiPoints"
> }
> 
> const file = app.vault.getAbstractFileByPath(dv.current().file.path)
> 
> async function spendKi(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.ki in fm)) fm[keys.ki] = 0
>     fm[keys.ki] = Math.max(0, fm[keys.ki] - amount)
>   })
> }
> 
> async function restoreKi(file, amount) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     if (!(keys.ki in fm)) fm[keys.ki] = 0
>     if (!(keys.max in fm)) fm[keys.max] = 0
>     fm[keys.ki] = Math.min(fm[keys.ki] + amount, fm[keys.max])
>   })
> }
> 
> async function resetKi(file) {
>   await app.fileManager.processFrontMatter(file, (fm) => {
>     fm[keys.ki] = fm[keys.max] ?? 0
>   })
> }
> 
> const spendBtn = dv.el('button', 'Spend Ki')
> spendBtn.onclick = () => spendKi(file, 1)
> 
> const restoreBtn = dv.el('button', 'Restore Ki')
> restoreBtn.onclick = () => restoreKi(file, 1)
> 
> const resetBtn = dv.el('button', 'Reset Ki')
> resetBtn.onclick = () => resetKi(file)
> 
> const current = dv.current()
> const ki = current[keys.ki] ?? 0
> const max = current[keys.max] ?? 0
> 
> dv.paragraph(`<h1> Ki Points: ${ki} / ${max} </h1>`)
> dv.span(spendBtn); dv.span(restoreBtn); dv.span(resetBtn)
> ```

> [!rests]-
> 
> > [!multi-column]
> > > [!short-rest]-
> > > ```dataviewjs
> > > const file = app.vault.getAbstractFileByPath(dv.current().file.path);
> > > const hitDieSize = 8;   // Adjust for class
> > > const conStat = 3;       // Constitution modifier
> > > 
> > > function shortRestWithHealing() {
> > >   const input = prompt("How many hit dice do you want to spend?");
> > >   const numDice = parseInt(input, 10);
> > > 
> > >   if (isNaN(numDice) || numDice <= 0) {
> > >     alert("Invalid input.");
> > >     return;
> > >   }
> > > 
> > >   let totalHealing = 0;
> > >   for (let i = 0; i < numDice; i++) {
> > >     const roll = Math.floor(Math.random() * hitDieSize) + 1;
> > >     totalHealing += roll + conStat;
> > >   }
> > > 
> > >   app.fileManager.processFrontMatter(file, (fm) => {
> > >     fm.currentHP = Math.min((fm.currentHP ?? 0) + totalHealing, fm.maxHP ?? 0);
> > >     fm.hitDice = Math.max((fm.hitDice ?? 0) - numDice, 0);
> > >   });
> > > 
> > >   alert(`You regained ${totalHealing} HP using ${numDice} hit dice.`);
> > > }
> > > 
> > > // Render the button
> > > const container = dv.el("div", "");
> > > const button = document.createElement("button");
> > > button.textContent = "🧘 Smart Short Rest";
> > > button.onclick = shortRestWithHealing;
> > > container.appendChild(button);
> > > ```
> >
> > > [!long-rest]-
> > > here

>[!multi-column]
>
>> [!strength]
>> **16** *(+3)*
>
>>[!dexterity]
>>**17** *(+3)*
>
>> [!constitution]
>> **16** *(+3)*
>
>>[!intelligence]
>>**10** *(+0)*
>
>>[!wisdom]
>>**16** *(+3)*
>
>>[!charisma]
>>**10** *(+0)*

> [!saving-throws] Saving Throws
> | Str | Dex | Con | Int | Wis | Cha |
> | --- | --- | --- | --- | --- | --- |
> | +6 | +6 | +3 | -1 | +3 | +0 |
> **Adv**: on INT/WIS/CHA against magic

> [!stats]
> *You have* ***2*** *action and* ***1*** *bonus action*
> > [!weapons]- Weapons List
> > | Attack            | +to hit | Damage                 | Range (ft) | Properties |
> > | ----------------- | ------- | ---------------------- | ---------- | ---------- |
> > | Unarmed Strike    | +6      | 1d6+3 Bludeoning       | 5          |            |
> > | Dagger            | +6      | 1d6+3 Piercing         | 20 (60)    |            |
> > | Dart              | +6      | 1d4+3 Piercing         | 20 (60)    |            |
> > | Javelin           | +6      | 1d6+3 Piercing         | 30 (120)   |            |
> > | Spear             | +6      | 1d6+3 (1d8+3) Piercing | 20 (60)    | Sap        |
> > | Hammer   | +6  | 1d8+3 bludgeoning  | 20 (60)   |     |
> > | Silver Morningstar       | +3      | 1d8+3  Piercing        | 5          | Sap        |
> > | Lightning Javelin | +6      | 1d6+3 (4d6 lightning)  | 30 (120)   |            |
> > | Shocking Grasp Spell Scroll Wraps | +6   | 2d8+3 lightning | Touch | Flurry of Blows only   |
> 
> > [!actions]- Bonus Actions List
> > **Deflect Attack**
> > When an attack with Bludgeoning, Piercing or Slashing damage hits you, you can reduce the attack’s damage by 1d10+7. If you reduce the damage to 0, you can redirect some of the energy. Target must succeed on Dex DC13 or take 2d6+3 damage of the same type the attack was with.
> > 
> > **Flurry of Blows**
> > Expend 1 Focus Point to make two Unarmed Strikes. If a creature is hit, you can impose one of the following effects:
> > - Addle - Target can’t make Opportunity Attacks until start of next turn
> > - Push - Target must succeed Str DC13 or be pushed up to 15ft away
> > - Topple - Target must succeed Dex DC13 or have the Prone condition
> >
> > **Patient Defense**
> > You can take the Disengage action as a bonus action, or expend a focus point to take both the Disengage and Dodge actions as a bonus action
> > 
> > **Step of the Wind**
> > You can Dash as a bonus action, or expend a focus point to Dash and Disengage as a bonus action, and double your jump distance for the turn
> > 
> > **Unarmed Strike**
> > Make an unarmed strike and pick one of the following effects:
> > - Damage - Make an attack roll against a creature
> > - Grapple - Target must succeed DC13 Str/Dex or it is grappled
> > - Shove - Target must succeed DC13 Str/Dex or is either pushed 5ft away or has Prone condition
> >
> > **Ki-Fueled Attack** 
> > If you spend 1 ki point or more as part of your action on your turn, you can make one attack with an unarmed strike or a monk weapon as a bonus action before the end of the turn.
> > 
> > **Stunning Strike** 
> > When you hit another creature with a melee weapon attack, you can spend 1 ki point to attempt a stunning strike. The target must succeed on a constitution saving throw or be stunned until the end of your next turn. DC 14

```tabs
--- Stats

|     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- |
| <input type="checkbox" checked id="ace1c4">| Acrobatics |  +6   | <input type="checkbox" unchecked id="b3cbcb"> | Medicine |  +3   |
| <input type="checkbox" checked id="ad224b">  | Animal Handling | +6 | <input type="checkbox" unchecked id="eec29a"> | Nature | 0 |
|<input type="checkbox" unchecked id="6db680">  | Arcana | 0 | <input type="checkbox" unchecked id="a9fd99"> | Perception | +3 |
| <input type="checkbox" checked id="9866c7"> | Athletics | +6 | <input type="checkbox" checked id="5e1824"> | Performance | +3 |
| <input type="checkbox" unchecked id="0ae2aa"> | Deception | 0 | <input type="checkbox" unchecked id="3d5963"> | Persuasion | 0 |
| <input type="checkbox" unchecked id="f53984">  | History | 0 | <input type="checkbox" unchecked id="d9a437"> | Religion | 0 |
| <input type="checkbox" unchecked id="0a90bc"> | Insight | +3 | <input type="checkbox" unchecked id="e7442a"> | Sleight of Hand | +3 |
| <input type="checkbox" unchecked id="61ea30">  | Intimidation | 0 | <input type="checkbox" unchecked id="5e96a9"> | Stealth | +3 |
| <input type="checkbox" unchecked id="8a4d45"> | Investigation | 0 | <input type="checkbox" checked id="5a6b85"> | Survival | +6 |

--- Monk Traits
<u> **Unarmoured Defense** </u>
*While you are wearing no armour and are not wielding a shield, your AC equls 10 + Dexterity modifier + your Wisdom modifier*

<u>**Martial Arts** </u>
*You gain the following benefits when unarmed or wielding only Monk weapons, provided you aren’t wearing armor or wielding a shield:*
- *You can use Dex. instead of Str. for the attack and damage rolls of your Unarmed Strikes and Monk Weapons. In addition, when you use the Grapple or Shove option of your Unarmed Strike, you can use Dex. instead of Str. to determine the save DC.*
- *You can make an Unarmed Strike as a Bonus Action.*
- *You can roll 1d6 in place of the normal damage from your Unarmed Strikes or Monk weapons.*

<u> **Monk’s Focus** </u>
*Your focus and martial training allows you to harness a well of energy within yourself called Focus Points. You have **5** Focus Points and regain all expended points after you finish a Short or Long Rest.*

*You can expend these points to enhance or fuel certain Monk features. You start knowing three such features:*

***Flurry of Blows***
*You can expend 1 Focus Point to make two Unarmed Strikes as a Bonus Action.*

***Patient Defense.***
*You can take the Disengage action as a Bonus Action or expend 1 Focus Point to take both the Disengage and Dodge actions as a Bonus Action.*

***Step of the Wind.*** 
*You can take the Dash action as a Bonus Action or expend 1 Focus Point to take both the Disengage and Dash actions as a Bonus Action, and your jump distance is doubled for the turn.*

*Features that use Focus Points may require your target to make a saving throw. The save DC equals 8 + Wis. modifier + Prof. Bonus.*

<u> **Ki-Fueled Attack** </u>
If you spend 1 ki point or more as part of your action on your turn, you can make one attack with an unarmed strike or a monk weapon as a bonus action before the end of the turn.

<u> **Stunning Strike** </u>
When you hit another creature with a melee weapon attack, you can spend 1 ki point to attempt a stunning strike. The target must succeed on a constitution saving throw or be stunned until the end of your next turn. DC 14

<u> **Dedicated Weapon** </u>
Whenever you finish a short or long rest, you can touch one weapon, focus your ki on it, and then count that weapon as a monk weapon until you use this feature again. The chosen weapon must meet these criteria:
- The weapon must be a simple or martial weapon
- You must be proficient with it
- It must lack the heavy and special properties

<u> **Uncanny Metabolism** </u>
*Once per Long Rest, when you roll Initiative you can regain all expended Focus Points, and regain 1d6+4 HP.*

<u> **Quickened Healing** </u>
As an action, you can spend 2 ki points and roll a Martial Arts die. You regain a number of hit points equal to the number rolled plus your proficiency bonus

<u> **Deflect Attacks** </u>
*When an attack roll that includes Bludgeoning, Piercing, or Slashing damage hits you, you can take a Reaction to reduce the attack’s total damage by 1d10 + Dex modifier + your monk level.*

*If you reduce the damage to 0, you can expend 1 Focus Point to redirect some of the attack’s force. If you do, choose a creature you can see within 5 ft. of you if it was melee, or a creature you can see within 60 ft. that isn’t behind Total Cover if the attack was ranged. The creature must succeed on a DC 13 Dex. saving throw or take 2d6+3 damage of the same type dealt by the attack.*

<u> **Focused Aim** </u>
When you miss with an attack roll, you can spend 1 to 3 ki points to increase your attack roll by 2 for each of these ki points you spend, potentially turning the miss into a hit.

<u> **Slow Fall** </u>
You can take a Reaction when you fall to reduce any damage you take from the fall by an amount equal to five times your Monk level.

<u> **Empowered Strikes** </u>
Whenever you deal damage with your Unarmed Strike, it can deal your choice of Force damage or its normal damage type. Your attacks count as magical for the purpose of overcoming immunity or resistance.

<u> **Wholeness of Body** </u>
You gain the ability to heal yourself. As a Bonus Action, you can roll your Martial Arts die. You regain a number of Hit Points equal to the number rolled plus your Wisdom modifier (minimum of 1 Hit Point regained).

You can use this feature a number of times equal to your Wisdom modifier (minimum of once), and you regain all expended uses when you finish a Long Rest.


--- Gnome Traits

**Darkvision**
Accustomed to life underground, you have superior vision in dark and dim conditions. You can see in dim light within 60 feet of you as if it were bright light, and in darkness as if it were dim light. You can’t discern color in darkness, only shades of gray.

**Gnome Cunning**
You have advantage on all Intelligence, Wisdom, and Charisma saving throws against magic

**Artificer’s Lore**
Whenever you make an Intelligence (History) check related to magic items, alchemical objects, or technological devices, you can add twice your proficiency bonus, instead of any proficiency bonus you normally apply.

**Tinker**
You have proficiency with artisan’s tools (tinker’s tools). Using those tools, you can spend 1 hour and 10 gp worth of materials to construct a Tiny clockwork device (AC 5, 1 hp). The device ceases to function after 24 hours (unless you spend 1 hour repairing it to keep the device functioning), or when you use your action to dismantle it; at that time, you can reclaim the materials used to create it. You can have up to three such devices active at a time.

When you create a device, choose one of the following options:

_Clockwork Toy._ This toy is a clockwork animal, monster, or person, such as a frog, mouse, bird, dragon, or soldier. When placed on the ground, the toy moves 5 feet across the ground on each of your turns in a random direction. It makes noises as appropriate to the creature it represents.

_Fire Starter._ The device produces a miniature flame, which you can use to light a candle, torch, or campfire. Using the device requires your action.

_Music Box._ When opened, this music box plays a single song at a moderate volume. The box stops playing when it reaches the song’s end or when it is closed.

--- Sub-class Traits
<u> **Rustic Hospitality** </u>
Since you come from the ranks of the common folk, you fit in among them with ease. You can find a place to hide, rest, or recuperate among other commoners, unless you have shown yourself to be a danger to them. They will shield you from the law or anyone else searching for you, though they will not risk their lives for you.

<u> **Drunken Technique** </u>
Whenever you use Flurry of Blows, you gain the benefit of the Disengage action, and your walking speed increases by 10 feet until the end of the current turn

<u> **Stumbling Performance** </u>
You gain proficiency in the Performance skill if you don't already have it. You also gain proficiency with brewer's supplies if you don't already have it.

<u> **Tipsy Sway** </u>

Starting at 6th level, you can move in sudden, swaying ways. You gain the following benefits.

**Leap to Your Feet** When you're prone, you can stand up by spending 5 feet of movement, rather than half your speed.

**Redirect Attack** When a creature misses you with a melee attack roll, you can spend 1 ki point as a reaction to cause that attack to hit one creature of your choice, other than the attacker, that you can see within 5 feet of you.

--- Proficiencies

|     |     |
| --- | --- |
| Armour    | None|
| Weapons | Crossbow, Hand, Scimitar, Shortsword, Simple weapons |
| Tools | Leatherworker's tools, Tinker's tools, Brewer's Tools, Vehicles (Land), Wargong |
| Languages | Common, Gnomish |


```



11600XP so far