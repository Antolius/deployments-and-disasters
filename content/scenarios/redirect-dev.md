---
title: "Redirect service developer handout"
description: handout for the Redirect service developer role in the Short.ly scenario
date: 2022-04-12T01:23:30+02:00
draft: true
---

You are a developer working on `🔄️ Redirect Service`. You came up with all the optimizations needed to get high throughput and low latency required for redirecting HTTP requests. **Your main responsibility is to keep redirects happening with lowest possible latency**.
<!--more-->

## What you know

{{< notice >}}
These are things that you know. They aren't secret, although you probably are the only one with this insight. You need to pay attention and share information with the team if and when it becomes relevant to the ongoing incident.
{{</ notice >}}

The redirect flow uses 3 tricks to achieve its throughput and latency requirements:

1. Commonly requested URLs are cached in `🔗️ Cache`. This cache keeps info about the URLs in memory, and is much faster that the traditional database. It is limited by the available RAM, so not every URL can be kept in cache at all times. URLs are added to cache only after they are redirected for the first time to save on space. `🔗️ Cache` uses the least recently used (LRU) eviction policy. This insures that URLs which are likely to be requested again soon remain cached. In practice around **80%** of redirect requests are resolved from the cache and only around **20%** require a database lookup.
1. All nonessential processing is postponed with the use of `📬️ Events Queue`. After finding the correct long URL `🔄️ Redirect Service` sends the information about the redirect event into the queue, and can respond to the user immediately. All nonessential things, like calculating and storing click rate statistics and even populating `🔗️ Cache` itself happens after the redirect response was already sent to the user.
1. Lastly, the micro-service architecture of the Short.ly's system allows for independent scaling of individual components. This means that while other services only have 2 instances, to insure upgrades without downtime, there are actually 3 instances of `🔄️ Redirect Service`. This helps it handle high loads.

## What you are good at

In the game you can perform any actions you want, but these are the ones you are good at. When performing these action you add `+2` to your dice rolls.

* **Checking application metrics and logs** from `🔄️ Redirect Service`. These include logs from the service itself, as well as metrics from the VM where the app is ruining.
* **Making changes to `🔄️ Redirect Service's` source code**.
* Service OPS, including **restarts, deployments and configuration updates**, such as tweaking logging levels, memory usage limits, etc.
* Checking metrics from and diagnosing issues with `🔗️ Cache` and `📬️ Events Queue`.
* Making manual updates to `🔗️ Cache` and `📬️ Events Queue`, including **querying the cache**, or **dropping messages from the queue**.
