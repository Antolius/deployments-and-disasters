---
title: "Turn"
date: 2020-02-16T23:29:03+01:00
draft: true
menu:
    side:
        weight: 30
---

The [incident stage]({{< ref "/play/session#incident-stage" >}}) of the Deployments and disasters session is made up of individual turns. Each turn represents a 5 minutes increment on the incident clock. A turn begins with disaster master revealing any new information. Players then discuss their options and agreeing on actions they indent to take. After that the dice are rolled to determine if actions succeed. A turn ends with DM narrating consequences of taken actions.
<!--more-->

## New information

At the start of each turn disaster master might introduce some new information. This might be some incoming communication, such as a CTO contacting the players on company rocket chat, or a client complaint submitted over e-mail. It might also be new automated alerts informing the players of a further degradation of service. This part of the turn is optional and DM decides hen to introduce it.

## Discussion

Each action has a difficulty determined by DM and expressed as a number, such as `7`, `13` or `20`. In each turn players have a common pool of 5 six-sided dice that they can assign to specific actions. Each player can attempt to perform at most one action, so number of actions per turn is limited to a number of players. Any number of dice (up to a total of 5) can be assigned to a given action, but each dice can only contribute to a single action.

In addition to difficulty each action involves using some technology or a tool. For example browsing application logs might be accomplished on Graylog. Querying database involves writing SQL queries. Pushing code updates makes use of git. Each player has a list of [tools they are proficient with]({{< ref "/play/roles#tool-proficiencies" >}}). When a player proficiency matches the tool used to perform a task they get a `+2` bonus to their dice roll. This is usually how players are assigned to actions. Just keep in mind that one player can't perform more that one action per turn.

In the discussion phase players brainstorm which actions to take, how many dice to assign to each action and who will perform it. While they are discussing actions they can consult with disaster master on the difficulty of each action that crosses their mind.

## Performing actions

Once players are happy with the actions they've chosen they lock in their decisions. After that they cannot change actions, players assigned to performing each of then or number of dice they've assigned to them.

Actions are then performed one by one by rolling assigned dices, summing up dice values, adding any proficiency bonus if applicable and comparing the sum with action difficulty. If the sum matches or exceeds the difficultly then the action succeeds. If sum is lower than difficulty the action fails. In each case disaster master explains consequences of performing an action.

{{< notice >}}
Since all actions are picked before they are resolved, a result of an action cannot influence other actions in the same turn.

This means that players need to wait for the next turn in order to react to a result of any action.
{{</ notice >}}

After all actions are performed the incident clock is advanced by 5 minutes.
