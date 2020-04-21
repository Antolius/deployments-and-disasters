---
title: "Scenario"
date: 2020-03-07T21:36:12+01:00
draft: true
menu:
    side:
        weight: 10
---
Incident scenario is the hart of every Deployments and disasters session. In it disaster master defines the software system that will encounter and incident and roles that players will fulfill.
<!--more-->

{{< notice >}}
It's usually best if you can adapt some actual incident that took place in your company into a game scenario. This will make it more engaging and meaningful to your players. Additionally, they will be able to learn about the real software system instead of made-up one.
{{</ notice >}}

## A case of who's done it

In its core an incident scenario is similar to a murder mystery. The disaster master comes up with a puzzle for players to solve and sprinkles clues into [extra information]({{< ref "/play/roles#extra-information">}}) provided to each of the roles.

When coming up with an incident the first thing to consider is how solvable it is. Think of your players and their skill set. Having an sub-optimal SQL query as a root cause of the incident will make it hard for forntend developers to figure out and resolve. Also try to keep the steps to resolve the incident clear once the root cause is uncovered. Figuring out what caused the incident will be an emotional high-point for your players. Dragging the resolution on or, what's worst, leaving players confused and without clear next steps will mess the game flow.

{{< notice >}}
Be careful when adapting real scenarios. Sometimes you will need to skip some details or leave out some contributing causes in order to keep the players focused.
{{</ notice >}}

## Leaving a trail

Starting with the solvable root cause the next step is figuring out how players will uncover it. There are two ways players receive insight into the software system:

* Information provided at the start of the session in their [role descriptions]({{< ref "/play/roles">}}).
* Information uncovered by their own [actions]({{< ref "/play/actions">}}).

DM should plan for hints to be uncovered from both of those sources. It's a good idea to split the information leading to the root cause into separate sources. For example:

* A business analyst might know upfront about a client increasing their app usage.
* Backend developer might know upfront that a given feature is under-tested.
* Telemetry lookup action will show a growing memory usage.

Players will have to look at all of those in order to assemble a complete story: increased usage hitting a peace of code with a memory leak is causing the app to crash.

It's usually enough to prepare two distinct peaces of starting information for each role. Not all of them need to lead in the direction of the root cause though. It's a good idea to add information on what routine system performance metrics look like. That will help players detect anomalies.

## Building out the system

When building out the software system that will experience the incident it's best to start with the real world system that the players work on. This might not always be possible; for example players might be coming from different companies. The next best thing is filling out the system with tools and technologies that are used in your company. For example, if your services are written in Go and metrics are stored in Prometheus use the same language and tool in your scenario.

Once you've designed a system decide on the what part of it will be handled by which role. This is a good moment to lock in the roles available in the scenario. Then write out the brief for each role. It's often useful to include system and flow diagrams to the briefs.