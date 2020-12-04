---
title: "Cooperate"
date: 2020-02-09T21:29:36+01:00
menu:
    side:
        weight: 30
---

## Motivation

Incident management process involves people from various roles. Those may include developers, ops engineers, support personnel, dedicated site reliability engineers, database administrators, etc. In order for incident to be resolved efficiently they may all need to work together.

In addition to coming from different backgrounds and being used to working in different conditions those roles also have different goals. For example support staff may be trying to send some kind of notice to clients that are suffering degraded performance. On the other hand, developers may be attempting to resolve the issues as soon as possible.

What's worst there can be inter-dependencies between different roles. For example support may need developers to provide them with a list of impacted clients or exact scope of issues they are experiencing. If developers are focused on removing the issue as fast as possible they may see support personnel and their request for information as a distraction.

This example illustrates the importance of good coordination and understanding of each others' roles and responsibilities.

## Metrics

There are two contexts that you can observe the players in: inside the session and afterwards in real world incidents.

During the session the host (disaster master) should observe the behavior of individual players. Assign them game roles that do not match their current job in the company. Track how they interact with other players.

For the aftereffects the disaster master cannot tract every player that ever played a session. You should lean on the player's managers or mentors for this continuing observation. Check in with line managers to collect feedback on players performance in real incidents a few months after they've played in a session. Pay attention to changes in their attitude.

Include cooperation on incidents into your company's stakeholders feedback if you can. For example if support personnel needs developers when crating the incident impact notice then support should be consulted as developer's stakeholders. Performance during incident management should be included in the regularly delivered feedback.