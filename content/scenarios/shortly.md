---
title: "Short.ly"
date: 2022-04-10T12:01:22+02:00
description: an incident scenario built around an URL shortening service
draft: true
menu:
    side:
        weight: 10
---

Short.ly is a fictional URL shortening SaaS company that is about to experience an incident. This scenario outlines the business model of the company and the system architecture of its service. It describes the incident timeline with all its contributing causes. It also provides a breakdown of [roles]({{< ref "/play/roles" >}}) that players can take up, as well as examples of [actions]({{< ref "/host/action_rules" >}}) for the Disaster Master.
<!--more-->

## About Short.ly

Short.ly is a relatively young company, but it has an established cross-functional development team, as well as customer success management team. Its business model aims for steady growth and early profitability without major investments. It adopted the micro-service architecture and agile methodology.

### Business model

Short.ly distinguishes between 2 types of users:
* Clients who generate short URLs, which need to have a Short.ly account. They can pay for access to metrics on the click rates on their URLs, or for longer retention of their URLs.
* End users, who click on short URLs and need to be redirected to the full URL. These users do not have accounts and aren't paying for the service.

The URL shortening itself is free but does require an account, so short URLs are always linked with the client who created it. Paying clients get click rates by user agent.

### System architecture

Short.ly uses a micro service architecture. This means there are separate services supporting each of the tree main features:
* Shortening URLs is performed by the `🗜️ Shortener service`. Info on the shortened URLs is stored in the `🔗️ URLs Database`. It is important that this flow works reliably. For example the short URL needs to be unique for each submitted URL. However, latency is less important in this case as request rates as lower.
* URL redirecting is handled by the `🔄️ Redirect Service`. It first looks for the URL in the `🔗️ Cache`, and falls back to the `🔗️ URLs Database` in case a short URL is not currently cached. Lastly it sends the info about the redirect event, including short and long URLs and the user agent info, to the `📬️ Events Queue`. Most requests on the system go through this flow, and it's important to retain low latency. This is why the `🔗️ Cache` component is important. It is also the reason why all followup processing of the redirect event happens asynchronously, and all that `🔄️ Redirect Service` needs to do is enqueue the event.
* Processing of redirect events is handled by the `⚙️ Event Handler` service, which reads from the `📬️ Events Queue`. It calculates click rates and stores them in the `📊️ Stats Database`. This used to happen only for paying clients at first, but was recently turned on for all clients to provide for an upsell opportunity. The `⚙️ Event Handler` also stores URL info into the `🔗️ Cache`. Performance of the `⚙️ Event Handler` service is less important as events are queued and that helps even the load in case of spike in redirect requests.

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

The services are highly available and independently scalable. There are at least 2 instances of each. Incoming API requests go through the `📣️ Load Balancer` which round robins across available service instances. There are three instances of the performance critical `🔄️ Redirect Service`, helping it handle the peak loads.

The `🔗️ Cache` cannot hold all the active URLs, however the 20-80 rule was found to effective enough. Approximately 20% of the daily active URLs are cached at any given time. The cache is limited by the available RAM and uses last recently used eviction policy. It ignores duplicates, so `⚙️ Event Handler` can just offer it URL from any redirect event it processes.

{{< notice >}}
The components of the Short.ly's system are intentionally kept generic. As a Disaster Master you should adapt them to your players. If you are hosting a session for your coworkers pick the language you use on your day job for the services. Likewise, if you work with concrete queue, cache or database implementation pick those. RabbitMQ, Kafka, Redis, Memcached, SQL or NoSQL, those are all OK options. It's also OK to keep the scenario generic, especially in case you have a mixed player group, or you can't gauge their skill level.
{{</ notice >}}

## The incident

The incident starts with an alert for latency increase on the redirect API endpoint. Soon after the API starts returning `503 Service Unavailable` error responses.

### Timeline

Lead up to the incident:
1. Click rates tracking is turned on for all clients in an effort to increase up sale. This results in a growing number of records in the `📊️ Stats Database`.
1. **Client 32** wants to track success rate of a marketing campaign. They work with Short.ly's sales team and come up with an idea of per-user URLs: each URL **client 32** shortens is different, resulting in a unique short URL with its own click rate.
1. **Client 32's** campaign results in even bigger growth of the `📊️ Stats Database`, which leads to degraded database performance.
1. With slower database updates the `⚙️ Event Handler` service slows down as well, causing the `📬️ Events Queue` to grow.

There is now a delay of several hours between when redirect event is enqueued and when it is processed by the `⚙️ Event Handler`.

The incident:
1. **Client 17** advertises a blog post using a short URL. Their post goes viral on social media.
1. There is a sharp increase in redirect requests for **client 17's** URL.
1. The initial redirect events are handled successfully, but their associated events get stuck in the `📬️ Events Queue` and are not yet cached.
1. Growing number of redirect requests hit the `🔗️ URLs Database`, causing performance issues. Redirect API latency rises.
1. 3 instances of the `🔄️ Redirect Service` cannot handle the load. Threads exhaustion results in `503 Service Unavailable` errors returned to users.

### Contributing causes

1. Low latency and high throughput for the redirect flow are identified as critical. As a result the `🔄️ Redirect Service` is heavily monitored. Since redirect event processing is behind the queue and does not directly impact API performance there is less monitoring in that part of the system. This is why performance issues with `⚙️ Event Handler` initially go undetected.
1. Inserting URLs into the `🔗️ Cache` is delegated to the part of the system which is considered less critical. However, having a fresh cache is key to the performance of the `🔄️ Redirect Service`.
1. While the `📊️ Stats Database` was initially well scaled, there were 2 events which caused increase in load that were not adjusted for. Tracking of all redirect events and the amount of different URLs generated by the **client 32** combined caused performance problems with the simplistic database design.
1. While the size of the `📬️ Events Queue` is tracked, there are no alerts associated with it. This is why the delay in processing of the queued events goes on unnoticed.
1. The system which is already on the brink of collapse is easily tipped over by the high load from **client 17's** viral blog post.

### Possible solutions

The quickest way to resolve the immediate issue with the **client 17's** viral blog post is to insert the URL into the `🔗️ Cache` manually. Depending on when players attempt this Disaster Master can either:
1. Consider this a resolution of the incident in case it's late in the game and wrap it up.
1. Introduce complications with other new URLs as `📬️ Events Queue` is now even bigger than in the beginning, since all those **client 17's** URLs are in there.

Another approach is to move updates to the `🔗️ Cache` from the `⚙️ Event Handler` into the `🔄️ Redirect Service`. This is technically challenging as it probably includes some coding and deployment. After players successfully develop and deploy the patch it completely resolves the incident. Discussion of how to optimize the `⚙️ Event Handler` can be held in the [review stage]({{<ref "/play/session#review-stage" >}}).

There are options between these two: like dropping some of the redirect events from the queue, or temporarily stopping the click rate tracking in the `⚙️ Event Handler`. Disaster Master should evaluate those suggestions based on:
* How late in the session the are proposed
* How detrimental they are to the Short.ly's business
* How technically challenging they are to perform

## Player roles

This scenario can be played by four to seven players. The roles include:

* **Customer success manager** They know about the **client 32's** per-user URLs. They are proficient in communication with clients, and can detect the viral post from **client 17** on social media.
* **Site reliability engineer** They know standard metrics, like usual API request rate, usual cache hit rate, etc. They are proficient in reading metrics and logs.
* **Developer** This role can be split into several depending on the number of players. For example, each developer in the team can be responsible for maintaining one of the tree flows. Alternatively, one developer can be in charge of the services, another is a DevOps and maintains the queue and the cache, while a third is a database admin. In either case each of them knows their part of the system, and is proficient in specific technologies used in it.

## Action suggestions

### Difficulty

### Complications
