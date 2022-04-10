---
title: "Roles"
date: 2020-02-09T23:50:18+01:00
description: what the disaster master and players should do
menu:
    side:
        weight: 10
---

In a session of Deployments and Disasters there are 2 kinds of participants:

* Disaster master
* Players
<!--more-->

## Disaster master

The disaster master (DM for short) is the one hosting the game. Their responsibility is to know the rules and prepare the incident scenario. Deployments and Disasters is a tabletop role playing game and outcomes of player actions are subject to chance. DM is the one setting difficulties of actions that players propose and determining consequences of actions they take. In addition to all this DM also prepares the pre-made roles that players pick from.

As you read through the rest of the rules DM's responsibilities will become more clear.

{{< rpg >}}
In tabletop RPGs this role is usually called game master.
{{</ rpg >}}

## Players

The rest of participants in the session are players. They are the ones attempting to resolve the incident by performing various actions. Deployments and Disasters is a cooperative game, so all players work together to solve the crisis.

Each player is assigned a specific role to play. The roles define:

1. Information about the system that the player receives at the start of the game.
1. Proficiencies with specific tools that they have.

{{< rpg >}}
The role roughly corresponds to a class in tabletop RPGs.
{{</ rpg >}}

Some examples of roles include:

* Backend developer
* DEVOPS engineer
* Support operative
* Site reliability engineer
* Frontend developer
* Database administrator
* Business analyst

{{< notice >}}
Role you get assigned in a particular session of Deployments and Disasters will likely differ from your actual job. This is OK. One of objectives of each session is to let you experience the incident from another person's perspective.
{{</ notice >}}

### Extra information

Depending on the role they pick, each player receives a slightly different overview of the software system. For example, a backend developer may know a bit more about various micro-services that make up the system and the business logic inside them. On the other hand site reliability engineer will know more about key performance metrics and system KPIs. Business analyst will have more information on the expected client behavior, etc.

At the start of the session each player should go over specific information provided by their role. Note that this information is not private or confidential in any way. In fact, the successful resolution of the incident often depends on a good [cooperation]({{< ref "/design/cooperate">}}) between players. Each player should constantly be thinking of how their insight might help the group and share it accordingly.

Additionally, players should feel free to share any real world knowledge about incident management procedures used in their company.

{{< rpg >}}
If you are used to other tabletop RPGs you might recognize this as meta-gaming. This practice is typically discouraged. However, in a session of Deployments and Disasters players are actually encouraged to meta-game. Remember that this is an educational activity with the goal of spreading knowledge about real world incident management tools and procedures.
{{</ rpg >}}

### Tool proficiency

In addition to extra information each player role gets a set of proficiencies. Think of them as technologies and tools that particular role is extra skilled with.

{{< rpg >}}
The term proficiency is commonly used in tabletop RPGs. The idea is the same here, with actual mechanical benefits being a simple +2 to dice rolls.
{{</ rpg >}}

For example, a backend developer might be proficient in Java programing language, Java Mission Control profiling tool, SQL, and Graylog. This signifies that a player in the role of backend developer has a higher chance to succeed in actions such as making updates to Java source code, profiling running Java service, or searching for particular logs in Graylog.

When a player performs an action with a tool they are proficient in they get a +2 to their dice roll. For more info on what exactly this means check out what makes an individual [game turn]({{< ref "/play/turn">}}).
