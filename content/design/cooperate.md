---
title: "Cooperate"
date: 2020-02-09T21:29:36+01:00
draft: true
menu:
    side:
        weight: 30
---

## Motivation

Incident management process involves people from various roles. Those may include developers, ops engineers, support personnel, dedicated site reliability engineers, database administrators, etc. In order for incident to be resolved efficiently they all need to work together.

In addition to coming from different backgrounds and being used to working in different ways those roles also have different goals. For example support staff may be trying to send some kind of notice to clients that are suffering degraded performance. On the other hand, developers may be attempting to resolve the issues as soon as possible.

What's worst there can be inter-dependencies between different roles. For example support may need developers to provide them with a list of impacted clients or exact scope of issues they are experiencing. If developers are focused on removing the issue as fast as possible they may see support personnel and that request them details on accounts as being in the way.

This example illustrates the importance of good coordination and understanding of each others roles and responsibilities.

## Metrics

There are two contexts that you can observe the players in: inside the session and afterwards, in real world incidents.

During the session the host (disaster master) should observe behaviors of individual players. Assign them game roles that do not match their current roles in the company. Track how they interact with other players.

For the aftereffects the disaster master cannot tract every player that ever played a session. You should lean on the player's managers (or mentors). Check in with line managers to collect feedback on players performance in real incidents a few months down the line after they've played the session. Track significant changes in attitude.

If you can include cooperation on incidents into your company's stakeholders feedback. Let's say support personnel needs developers when crating the incident impact notice. Then support should be included as developer's stakeholders. And performance during incident management should be included in the regularly collected feedback.