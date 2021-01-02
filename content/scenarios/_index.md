---
title: "Incident scenarios"
date: 2020-02-09T20:19:03+01:00
description: an example scenario involving an API/Queue/DB interaction
draft: true
menu:
    main:
        weight: 30
---
Coming soon...
<!--more-->
{{< mermaid >}}
graph TD
    subgraph API layer
        API[API gateway] --> Queue([Actions queue])
        Queue --> Taker[Actions taker]
    end
    subgraph Backend layer
        API --> Auth[Auth. service]
        Liker[Likes service] --> Auth
        Liker --> DB[(Likes store)]
        Taker --> Liker
        Taker --> Other{{Other services}}
    end
{{</ mermaid >}}