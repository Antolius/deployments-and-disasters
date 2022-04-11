---
title: "Shortener service developer handout"
description: handout for the Shortener service developer role in the Short.ly scenario
date: 2022-04-11T23:58:12+02:00
draft: true
---

You are a developer working on the `🗜️ Shortener service` and maintaining the `🔗️ URLs Database`. You implemented the algorithm for generating short URL from the client provided long URL. You are responsible for keeping the shortening API up and running.
<!--more-->

## What you know

{{< notice >}}
These are things that you know. They aren't secret, although you probably are the only one with this insight. You need to pay attention and share information with the team if and when it becomes relevant to the ongoing incident.
{{</ notice >}}

Clients submit URLs for shortening via a `POST` request to `https://api.short.ly/v1/urls` endpoint. The long URL is sent in the JSON formatted request body. They provide their credentials in the `Authorization` request header. A short URL is returned in the JSON formatted response body. For example a request:

```
POST /v1/urls HTTP/1.1
Host: api.shot.ly
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
Content-Type: application/json
{
    "url": "https://example.com/some/long/url"
}
```

would receive a response like this:

```
HTTP/1.1 201 Created
Content-Type: application/json
{
    "shortUrl": "https://short.ly/wN+jBj"
}
```

All client requests first go through the `📣️ Load Balancer`, which sends them on to instances of `🗜️ Shortener service`, of which there are two. This enables you to deploy new versions of the service without downtime.

`🗜️ Shortener service` handles API requests like this:

{{<mermaid>}}
flowchart TB
    A[/API request/] --> B{Authenticated?}
    B --NO--> C([Return 401 Unauthorized HTTP status])
    B --YES--> D[/Append client ID to the long URL\]
    D --> E{{Produce MD5 hash of the client ID + URL}}
    E --> F{{Encode the hash in base64}}
    F --> G{{Pick 6 random characters as short code}}
    G --> H[[Insert short code, long URL and client ID into DB]]
    H --> I{Is short code unique?}
    I --NO--> G
    I --YES--> J([Return 201 Created HTTP status with short URL in response body])
{{</mermaid>}}

The `urls` table in the `🔗️ URLs Database` looks like this:

|       | Column name   | Type        |
|-------|---------------|-------------|
| 🔑️ PK | short_code    | varchar(16) |
|       | full_url      | nvarchar    |
|       | creator_id    | int         |
|       | created_at    | datetime    |

## What you are good at

In the game you can perform any actions you want, but these are the ones you are good at. When performing these action you add `+2` to your dice rolls.

* Checking application metrics and logs from the `🗜️ Shortener service`. These include logs from the service itself, as well as metrics from the VM where the app is ruining.
* Service OPS, including restarts, deployments and configuration updates, such as tweaking logging levels, memory usage limits, etc.
* Making changes to the `🗜️ Shortener service's` source code.
* Checking metrics, diagnosing issues and analyzing query execution plans from the `🔗️ URLs Database`.
