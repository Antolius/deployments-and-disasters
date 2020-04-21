---
title: "Action rules"
date: 2020-03-08T21:10:38+01:00
draft: true
menu:
    side:
        weight: 20
---
[Actions]({{< ref "/play/actions" >}}) are aspect of the session where player creativity comes into a forefront. It's up to players to come up with actions, and up to the disaster master to specify their difficulty and narrate successful or failed outcomes. However, nudging players into taking the right actions might be the most subtle and complex way for a DM to influence a session.
<!--more-->

## Setting difficulty

It should generally suffice to sort actions into three categories: easy, medium and hard. Those should, on average, be solvable by a player with tool proficiency (+2 bonus) and 1, 2 or 3 dice respectively. More precisely:

|Action|Difficulty|
|------|----------|
|Easy  |7         |
|Medium|10        |
|Hard  |13        |

Sometimes players will fail to perform an action in one turn and will retry it the next. In this case it's a good idea to lower the difficulty rating. It often makes sense in the narrative. For example, if player is going over a thread dump looking for clues, the more time they spend on it the more likely they'll find them. It also helps the session progress.

{{< rpg >}}
Similarly to tabletop RPGs, a DM should not allow players to make meaningless actions. Each action should, if it succeeds, help the players. For example it might help them eliminate some wrong presumptions, help them uncover the root cause of the incident or decreasing its impact.
{{</ rpg >}}

## Narrating results

The action succeeds if dice roll + proficiency bonus matches or exceeds the difficulty. In that case either offer useful information to the players or let them know if something beneficial happened. Strictly speaking positive side effects might not be immediately obvious. However, requiring extra action on the next turn to confirm a previous action worked is bad for pacing. Oftentimes a successful action can be hinted by letting players know that some error based alert has closed.

In case action fails things become more interesting. Depending on the type of the action there are two ways of handling failure:

* In case of [passive action]({{< ref "/play/actions#passive-actions" >}})  DM just withholds information from players.
* In case of [active action]({{< ref "/play/actions#active-actions" >}}) DM introduces a complication.

## Complications

Disaster master can choose to introduce a complication at the beginning of a [turn]({{< ref "/play/turn" >}}), or in response to a failed active action. IN general there are two types of complications:

* Interrupts that require a low difficulty (i.e. 3) passive action to resolve.
* Degradations in the software system that might spread the impact of the incident, or make further actions more difficult.

### Interrupts

Interrupts work well as complications to randomly introduce at the beginning of a turn. They might be CTO contacting the team and requesting an update, or a client submitting an complaint and asking for some assistance. In both cases DM lets players know that they can answer the interrupter with a low difficulty passive action.

{{< notice >}}
DM should be ready to introduce additional complications in case players choose to ignore an interrupt.
{{< /notice >}}

Use interrupts to slow the players down a bit.

### Degradations

Degradations represent worsening situation. They might be caused by some player actions, for example a failed data base query might lock up a table, or a failed code deployment might make one app instance inaccessible. These make for a good complication to follow a failed action attempt.

Alternatively, degradations might be brought on by changing conditions such as client behavior or slowly draining resources. This type of degradation can be introduced as complication at the beginning of a turn. Use this type of complication to focus players' attention on a certain part of the system.

## Managing players

Each game turn represents 5 minutes of _in game_ time. However, it will likely take players longer to agree on the action to take. In general DM should let players brainstorm for 5 to 10 minutes. If discussion starts dragging on, DM can jump in with a run-down of discussed options and outline difficulty of each one. This can help players consider all possibilities and narrow down their choices.

DM can also use this summarizing of options to bring more introverted players into focus. To do this just start with suggestions of one of your quieter players This will help surface their ideas.
