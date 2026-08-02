---
title: "Client-Side Load Balancing at a Million Requests Per Second"
url: "https://engineering.zalando.com/posts/2026/06/client-side-load-balancing.html"
date: "2026-06-23"
author: "Conor Gallagher"
feed_url: "https://engineering.zalando.com/atom.xml"
---
How we built an in-process client-side load balancer for a million requests per second of internal fan-out traffic, what we layered on top (N-ring fade-in, occupancy-based bounded load, and AZ-aware routing with a latency health factor), and how hardening that path cut cost and made the service resilient to the infrastructure underneath it.
