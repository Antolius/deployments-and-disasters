---
title: "Redirect service developer handout"
description: handout for the Redirect service developer role in the Short.ly scenario
date: 2022-04-12T01:23:30+02:00
draft: true
---

You are a developer working on `🔄️ Redirect Service`. You came up with all the optimizations needed to get high throughput and low latency required for redirecting HTTP requests. You are responsible for keeping `🔄️ Redirect Service` up and running with lowest possible latency.
<!--more-->

## What you know

{{< notice >}}
These are things that you know. They aren't secret, although you probably are the only one with this insight. You need to pay attention and share information with the team if and when it becomes relevant to the ongoing incident.
{{</ notice >}}

The redirect flow uses 3 tricks to achieve its throughput and latency requirements:

1. Commonly requested URLs are cached in `🔗️ Cache`. This cache keeps info about the URLs in memory, and is much faster that the traditional database. It is limited by the available RAM, so not every URL can be kept in cache at all times. URLs are added to cache only after they are redirected for the first time to save on space. `🔗️ Cache` uses the least recently used (LRU) eviction policy. This insures that URLs which are likely to be requested again soon remain cached. In practice around **80%** of redirect requests are resolved from the cache and only around **20%** require a database lookup.
1. All nonessential processing is postponed with the use of `📬️ Events Queue`. After finding the correct long URL `🔄️ Redirect Service` sends the information about the redirect event into the queue, and can respond to the user immediately. All nonessential things, like calculating and storing click rate statistics and even populating `🔗️ Cache` itself happens after the redirect response was already sent to the user.
1. Lastly, the micro-service architecture of the Short.ly's system allows for independent scaling of individual components. This means that while other services only have 2 instances, to insure upgrades without downtime, there are actually 3 instances of `🔄️ Redirect Service`. This helps it handle high loads.

This is how a request to a short URL is processed:

{{<mermaid>}}
sequenceDiagram
  actor user as User
  participant lb as 📣️ Load Balancer
  participant rs as 🔄️ Redirect Service
  participant cache as 🔗️ Cache
  participant db as 🔗️ URLs Database
  participant queue as 📬️ Events Queue
  user->>lb: GET https://short.ly/ie3Sfk, User-Agent: Chrome
  lb->>rs: GET https://rs.local/ie3Sfk, User-Agent: Chrome
  rs->>cache: GET ie3Sfk
  note over rs,cache: First look for URL in the cache.
  alt This happens in case URL is present in the cache (~80% or requests)
      rect rgb(253, 246, 227)
      cache->>rs: {"id": "ie3Sfk", "url": "https://example.com/long/url", "cretor_id": 12}
      rs->>lb: 301 Moved Permanently, Location: https://example.com/long/url
      lb->>user: 301 Moved Permanently, Location: https://example.com/long/url
      rs-)queue: $timestamp, 'Chrome', 'ie3Sfk', 'https://example.com/long/url', 12
      note over rs,queue: Enqueue redirect event so it can be cached and request rate can be updated.
      end
  else This happens in case URL is absent from the cache (~20% or requests)
      rect rgb(253, 246, 227)
      cache->>rs: null
      rs->>db: SELECT url, creator_id FROM urls WHERE id='ie3Sfk'
      note over rs,db: Fall back to looking for URL in the database.
      alt This happens in case URL is present in the database
          rect rgb(238, 232, 213)
          db->>rs: https://example.com/long/url, 12
          rs->>lb: 301 Moved Permanently, Location: https://example.com/long/url
          lb->>user: 301 Moved Permanently, Location: https://example.com/long/url
          rs-)queue: $timestamp, 'Chrome', 'ie3Sfk', 'https://example.com/long/url', 12
          note over rs,queue: Enqueue redirect event so it can be cached and request rate can be updated.
          end
      else This happens in case URL is absent from the database
          rect rgb(238, 232, 213)
          db->>rs: null
          rs->>lb: 404 Not Found
          lb->>user: 404 Not Found
          end
      end
      end
  end
{{</mermaid>}}

## What you are good at

In the game you can perform any actions you want, but these are the ones you are good at. When performing these action you add `+2` to your dice rolls.

* Checking application metrics and logs from `🔄️ Redirect Service`. These include logs from the service itself, as well as metrics from the VM where the app is ruining.
* Service OPS, including restarts, deployments and configuration updates, such as tweaking logging levels, memory usage limits, etc.
* Checking metrics from and diagnosing issues with `🔗️ Cache` and `📬️ Events Queue`.
* Making manual updates to `🔗️ Cache` and `📬️ Events Queue`, including querying the cache, or dropping messages from the queue.
