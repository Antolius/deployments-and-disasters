---
title: "Actions"
date: 2020-02-22T23:09:23+01:00
menu:
    side:
        weight: 40
---

There are 2 types of actions in a game of Deployments and disasters:

1. **Passive actions** in which players collect information from the software system without changing it.
1. **Active actions** in which players make changes to the system or interact with it in an active capacity.
<!--more-->

## Passive actions

Examples of passive actions include:

* Looking up metrics on Grafana.
* Searching for logs on Graylog.
* Going through code, looking for bugs.
* Receiving alerts on mail.
* Answering CTO's questions.
* Sending the client impact report.

One common characteristic of passive actions is that they can't directly impact the software system. These actions are mostly used for collecting information on the system. They also include interacting with other stakeholders, such as clients or management.

When players succeed in a passive action they will usually gain extra information on the system. If they fail they will not make the incident worse.

## Active actions

Examples of active actions include:

* Updating the production configuration.
* Deploying new application versions.
* Reverting to previous versions.
* Querying production database.
* Disabling specific user accounts.
* Taking a thread / heap dump from running app.

Active actions change the state of the software system. They usually represent players attempting to resolve the incident or mitigate its impact. Note that running database queries and generating telemetry form running applications also falls into this category. This is because those actions run in the context of the production app and can have an impact on it.

Active actions result in some kind of a change to the software system. Failed actions can have negative impact on the incident such as extending the impact to new clients or functionalities. Think of a badly written database query that locks a table, or a heap dump that fills up hard disc and causes the app to crash.

## Tools / technologies

Each action relies on some tool or technology. Tools may include:

* Graylog
* Grafana
* Prometheus
* Java mission control
* Git
* In-house tool for client management

Technologies may include programing languages, but also languages like SQL, or configuration file formats such as YAML. Lastly, an action might involve interacting with particular individual or groups of stakeholders.