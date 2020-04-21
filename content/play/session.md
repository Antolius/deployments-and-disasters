---
title: "Session"
date: 2020-02-16T23:28:57+01:00
draft: true
menu:
    side:
        weight: 20
---

Each session of Deployments and disasters is about resolving a single incident. A game session has 3 stages:

1. **Briefing stage**, where DM explains the scenario they've prepared and players pick roles.
1. **Incident stage**, where DM introduces the incident and players take actions to resolve it.
1. **Review stage**, where players discuss future proofing the system.
<!--more-->

[Disaster master]({{< ref "/play/roles.md#disaster-master" >}}) defines the software system that will suffer the incident and prepares [player roles]({{< ref "/play/roles.md#players" >}}). It's generally hard to squeeze several incidents into the same system and keep it compact enough for players to quickly grasp. That's why each Deployments and disasters session takes place in a standalone, separate scenario.

{{< rpg >}}
In tabletop RPG terms this makes Deployments and disasters session a one-off.
{{</ rpg >}}

## Briefing stage

The main objective of the briefing stage is for all the players to familiarize themselves with some basic information. That includes:

1. Rules of the Deployments and disasters game itself.
1. Technical and business aspects of the scenario that DM prepared.
1. In-dept info on the software system that their role provides.

If players are from different teams or departments (or different companies all together) this is the time for everyone to get to know each other.

## Incident stage

Incident stage begins with DM introducing the actual incident in the software system described in the briefing stage. After that incident stage is divided into individual turns. On each turn players can perform any number of actions in an attempt to understand what caused the incident and to resolve it.

The goal of this stage is to resolve the incident in as few turns as possible. This is tracked by the incident clock that starts at 00:00 and progresses by 5 minutes each turn. Depending on the scenario DM marks certain key timestamps on the clock, such as duration after which penalties are payed to clients, or duration that denotes the breach of contract. Breach of contract usually stands for players loosing the game.

Players can gain some extra time (effectively push timestamps back) by publishing a detailed client impact by the end of the 3rd turn. The report should include:

* A list of impacted clients.
* A list of impacted features.
* The nature of impact (increased latency or error rates, drop of throughput, etc.).

## Review stage

This stage is modeled after your company's post incident review. Here's where players get to discuss:

1. All the factors that contributed to the incident.
1. What actions were performed in order to resolve the incident.
1. What can be done to prevent the incident from repeating.
1. What can be done to improve the incident management process itself.

If the company uses some standardized reporting format for this type of meeting players can fill out a mock version of it.

In addition to post incident review in this stage disaster master can collect some feedback on the game itself. This can be useful for [scaling the game]({{< ref "/design/scale#measure" >}}).