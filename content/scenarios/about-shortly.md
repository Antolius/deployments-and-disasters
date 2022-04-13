---
title: "About Short.ly handout"
description: description of a fictional Short.ly startup and its software architecture
date: 2022-04-12T23:54:12+02:00
draft: true
---

Short.ly is a relatively young company, but it has an established cross-functional development and customer success management team. Its business model aims for steady growth and early profitability without major investments. It employs a micro-service architecture and agile methodology.
<!--more-->

## Business model

Short.ly distinguishes between 2 types of users:
* Clients who generate short URLs, which need to have a Short.ly account. Shortening URLs is free but they can pay for redirect rate statistics.
* End users, who click on short URLs and need to be redirected to the full URL. These users do not have accounts and aren't paying for the service.

## System architecture

Short.ly uses a micro service architecture. This means there are separate services supporting each of the tree main features:
* Shortening URLs is performed by `🗜️ Shortener service`. Info on the shortened URLs is stored in `🔗️ URLs Database`.
* URL redirecting is handled by `🔄️ Redirect Service`. It first looks for the URL in `🔗️ Cache`, and falls back to `🔗️ URLs Database` in case a short URL is not currently cached. Lastly it sends the info about the redirect event, including short and long URLs and the user agent, to `📬️ Events Queue`.
* Converting redirect events into click rate statistics is handled by `⚙️ Event Handler` service, which reads from `📬️ Events Queue`. It calculates click rates and stores them in `📊️ Stats Database`. It also populates `🔗️ Cache` with short codes and long URLs.

{{<mermaid>}}
flowchart TB
    users(("👥️ users")) <--> lb["📣️ Load Balancer"]
    lb <--> shs[["🗜️ Shortener service 1, 2"]]
    shs --> db[("🔗️ URLs Database")]
    lb <--> rs[["🔄️ Redirect Service 1, 2, 3"]]
    rs <--> cache[("🔗️ Cache")]
    rs <-.-> db
    rs --> queue(["📬️ Events Queue"])
    queue --> eh[["⚙️ Event Handler 1, 2"]]
    eh --> cache
    eh --> stats[("📊️ Stats Database")]
{{</mermaid>}}
