---
title: "Site reliability engineer handout"
description: handout for the client success manager role in the Short.ly scenario
date: 2022-04-11T20:53:32+02:00
draft: true
---

You are a site reliability engineer (SRE) at Short.ly. You work with the dev team to define key metrics to track and performance thresholds that services need to meet. You are in charge of setting up alerting on top of those metrics. It's your job to make sure that all the components of Short.ly's architecture are scaled correctly.
<!--more-->

## What you know

{{< notice >}}
These are things that you know. They aren't secret, although you probably are the only one with this insight. You need to pay attention and share information with the team if and when it becomes relevant to the ongoing incident.
{{</ notice >}}

When choosing metrics to focus on you want them to be as representative of clients' experience as possible. In Short.ly's micro service architecture `📣️ Load Balancer` is the first component that clients' requests reach. This makes metrics such as latency collected from this component closest to what clients are seeing.

### SLIs

Service level indicators are the key metrics that track the health of the system. In Short.ly's case those are:

1. Average latency of redirect responses (i.e. how long it takes Short.ly servers to respond to HTTP requests to short URLs).
1. 99th percentile latency of redirect responses.
1. Ratio of successful redirect responses (those with HTTP status codes `304` or `404`) out of total request rate on the short URLs.
1. Ratio of successful redirect responses (those with HTTP status codes `200` or `401`) out of total request rate on the URL shortening API.

The latency is only important for the redirects. Clients tolerate higher latency on the shortening API. Successful processing of HTTP requests, on the other hand, is important for both flows. Both latency and success rate metrics are measured on `📣️ Load Balancer`.

### SLOs

Service level objectives are performance targets that Short.ly system need to meet. In particular:

| SLI (the metric)                     | SLO (the target value) |
|--------------------------------------|------------------------|
| average redirect latency             | < 10 ms                |
| 99th percentile redirect latency     | < 100 ms               |
| error ratio of redirect responses    | < 0.1% per hour        |
| error ratio of shorten API responses | < 1% per hour          |

These objectives are reflected in various contracts that Short.ly has with clients, codified in service level agreements (SLAs). If these agreements are breached Short.ly is liable to pay penalties to clients.

### Scalability

Components of the Short.ly system are scaled according to the expected load. For example, here are expected query/request per second (QPS/RPS) rates for HTTP requests:

|         | shortening API | redirects    |
|---------|----------------|--------------|
| Average | 200 req/s      | 20 000 req/s |
| Peak    | 400 req/s      | 50 000 req/s |

Before going into production the system was tested with these loads, and it functioned well. You've also seen actual production request rates approach these peak expected rates, and system performed within proscribed SLOs.

## What you are good at

In the game you can perform any actions you want, but these are the ones you are good at. When performing these action you add `+2` to your dice rolls.

* Querying metrics and logs from `📣️ Load Balancer`. This includes latency and request rates segmented by HTTP response status or requested short URL.
* Checking capacity related metrics, such as free / used up disc space on database servers, memory usage of `🔗️ Cache` and `📬️ Events Queue`, number of redirect events in the queue.
* Setting up new application VMs to scale existing services.
