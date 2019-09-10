# Deployments & Disasters rules

Rules for playing a game of Deployments & Disasters. This document is intended for players going into their first game, or looking to refresh their memory.

## Overview

Deployments & Disasters is intended to be used as both a fun pastime and a training tool to get us all better at incident management. Games are intended to be played as one-off sessions, not continuous campaigns. Each game centers around resolving a single incident impacting a single software system.

A game session is composed of 3 stages:

1. Briefing stage. Here players are assigned roles and introduced to the system that will experience failure. Each player is given a set of tool proficiencies.
1. Incident stage. This is where the main event takes place. Players attempt to resolve the incident. DM gets in their way. Good stuff.
1. Aftermath stage. Here players discuss future proofing the system. At the end they provide meta feedback on the game itself.

Main objective of the game is to resolve the incident (2nd stage) as fast as possible. Incident stage is composed of individual turns. Each turn has 2 steps:

1. Planning step. Players decide who among them will take what actions. DM reveals the difficulty of completing each action. Players allocate D6 dice from a shared 5 dice pool to each action.
1. Execution step. Players roll the dice, and apply tool proficiency bonus. If dice + bonus is strictly larger than difficulty the action succeeds. DM reveals the consequences of each action.

The goal of the game is to resolve the incident in as few turns as possible. This is tracked by the incident clock that starts with the incident stage and progresses by 5 minutes per turn. Depending on the scenario DM marks certain key timestamps on the clock, such as duration after which penalties are payed to clients, or duration that denotes the breach of contract.

Players can gain some extra time (push timestamps back) if they publish a detailed client impact within the first 3 turns of the game. The report should include the list of impacted clients, and features, as well as the nature of impact.

## Anatomy of the turn

As noted, the incident resolution plays out as a series of turns. In each turn players first decide on actions they will take, and then all the actions are resolved. Since all actions are picked before any of them are resolved players need to wait until the next turn to react to them.

There are a few rules on formulating the action. Each action falls strictly into one of 2 categories:

1. Query action. Result of this is some insight into the system. If action fails DM will withhold some or all of the queried information.
1. Command action. Result of this is some change in the system. If action fails DM will introduce extra complication to the scenario.

In addition to a category, each action has one tool that's used to accomplish it. For query actions those can be systems used to query logs / metrics. For command actions those are usually OPS tools for operations like deployment, programming languages for changes to the code, DB management apps for updates to stored data, etc. 

Players can define any action they wish to perform, as long as they stick to one category & tool per action. When defining actions players should try to be as precise as possible since DM has the right to interpret ambiguous situations.

Turn starts with the planning step. The goal here is to define which players will attempt which actions and allocate a certain number of dice to them. Players should reach a common agreement on what actions to pick. As they discuss actions they check with DM on the difficulty of each action. Each player has proficiency in using certain tools. If player proficiency matches the tool used to accomplish an action they receive a bonus of `+2` to the total dice roll. Each player can attempt at most one action per turn, and not all players must act. Players split 5 D6 dice among their actions, informed by their difficulty.

After the planning phase players roll the dice for each action in sequence. Dice rolls and bonus are added up, and DM reveals the result of the action. In case of a query action DM will reveal or withhold information. In case of command action they will narrate the results, optionally introducing a complication.

### Example

Let's imagine 3 players:

* Fred, a frontend dev with proficiency in TypeScript, Node.js, Angular and NewRelic.
* Becky, a backend dev with proficiency in Java, Spring, SQL and JMC.
* Sasha, a site reliability engineer with proficiency in Prometheus, Graylog, Grafana and Kubernetes.

They are trying to fix their Todo list app after clients reported _"having problems with gosh darn web app"_. It's now turn 3 and they have previously figured out that the problem is related to the completed tasks page, which fails to render.

Players discuss potential actions. They are thinking of devoting one action to sending a client impact announcement. They know that completed tasks feature is impacted, but are not sure if all clients are affected. Unfortunately they don't have any more time to investigate before sending the notice. DM estimated the difficulty of informing correct users is 8.

They are also thinking about further troubleshooting. Fred suggests the problem might be on the API side, since he knows that completed tasks component uses a unique combination of API calls to resolve view permissions. Becky suggests looking for specific authorization logs from the API backend app. Players are not sure what exactly to look for among the logs so they agree to look for "any anomalies". DM informs them that anomaly detection is a science in itself, and estimates difficulty of this action is 10.

Players agree to take 2 actions. Fred will perform a command action and send the report to clients using the company CRM tool. They will allocate 3 dice to this, since they really need the extra time on the clock this action will afford them. Sasha will perform a query action of searching for security logs on Graylog, since she has proficiency in that tool. They allocate the remaining 2 dice to this action.

With the planning over, they enter the execution step. Players roll the dice for client notification action. They get `3 + 3 + 4 == 10` which is greater than the difficulty of 8. The action passes and they inform the right clients of the outage. DM moves the breach of contract timestamp back by 30 minutes with gives them 6 more turns to resolve the incident. Relief around the table is palpable. They they roll the remaining 2 dice, and get `5 + 1 == 6`, plus bonus: `6 + 2 == 8`. Since this is not greater than the difficulty of 10, this action fails. However, because they came close DM decides to disclose a bit of info and informs them that while they did not find the root of the problem, they did see an increase in number of security logs. This intrigues the players, and they start planning for the next turn. DM sinisterly advances the incident clock by 5 minutes. 

