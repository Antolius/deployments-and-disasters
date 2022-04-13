---
title: "Event Handler service developer handout"
description: handout for the Event Handler service developer role in the Short.ly scenario
date: 2022-04-12T23:08:11+02:00
draft: true
---

You are a developer working on `⚙️ Event Handler` service and maintaining `📬️ Events Queue` and `📊️ Stats Database`. These are important, as click rate statistics are what clients are paying from. **Your main responsibility is to make sure all paying clients get accurate click rate statistics at the end of the month.**
<!--more-->

## What you know

{{< notice >}}
These are things that you know. They aren't secret, although you probably are the only one with this insight. You need to pay attention and share information with the team if and when it becomes relevant to the ongoing incident.
{{</ notice >}}

Since redirect event processing happens downstream from `📬️ Events Queue` its performance does not impact either redirects or shortener API performance. For now clients do not interact with `📊️ Stats Database` directly. Instead, we regularly export weekly and monthly reports and send them to clients. There are plans for automating this, but it isn't a priority just yet.

Each redirect request made from a browser comes with a `User-Agent` header. It includes information on which browser was used to make the request, but also which OS it came from. This information can get quite specific, for example:

* `Mozilla/5.0 (Linux; Android 4.0.4; Galaxy Nexus Build/IMM76B)
AppleWebKit/535.19 (KHTML, like Gecko)
Chrome/18.0.1025.133 Mobile Safari/535.19`
* `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36 Edg/91.0.864.59`
* `Mozilla/5.0 (iPad; U; CPU OS 3_2_1 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Mobile/7B405`

Redirect rates are stored in `redirects` table in `📊️ Stats Database`, which looks like this:

|       | Column name   | Type        |
|-------|---------------|-------------|
| 🔑️ PK | short_code    | varchar(16) |
| 🔑️ PK | user_agent    | nvarchar    |
|       | full_url      | nvarchar    |
|       | client_id     | int         |
|       | click_count   | bigint      |

`short_code` and `user_agent` columns make up a composite primary key. The `click_count` column is updated each time redirect is requested for a short URL from the same user agent. `client_id` and `full_url` are auxiliary data kept for convenience to make the generation of reports easier.

Until recently redirect rates were only calculated for paying clients. However, this changed and rates are now calculated for redirect events. This simplified already straightforward logic in `⚙️ Event Handler` service. The flow for handling events is now straightforward:

1. Redirect event is read from `📬️ Events Queue`.
1. If record in `redirects` table already exists for the combination of `short_code` and `user_agent` then its `click_count` is incremented. If record doesn't yet exist it is created.
1. `short_code`, `full_url` and `client_id` are sent to `🔗️ Cache`. Cache itself handles deduplication, so handler can just send data every time and not worry about it.

## What you are good at

In the game you can perform any actions you want, but these are the ones you are good at. When performing these action you add `+2` to your dice rolls.

* **Checking application metrics and logs** from `⚙️ Event Handler`. These include logs from the service itself, as well as metrics from the VM where the app is ruining.
* **Making changes to `⚙️ Event Handler's` source code**.
* Service OPS, including **restarts, deployments and configuration updates**, such as tweaking logging levels, memory usage limits, etc.
* Checking metrics from and diagnosing issues with `📬️ Events Queue`.
* Making manual updates to `📬️ Events Queue`, including **peeking or dropping messages from the queue**.
