---
title: "Thundering herd of circuit breakers"
url: "https://engineering.zalando.com/posts/2026/10/thundering-herd-of-circuit-breakers.html"
date: "2026-10-08"
author: "Valeria Ponyaeva"
feed_url: "https://engineering.zalando.com/atom.xml"
---
A client-side circuit breaker stays open for hours, blocking 95% of traffic, and won't close. Our service shows no sign of being unhealthy. To understand the anomaly, we build a custom span visualizer on top of OpenTelemetry trace data and find a circuit breaker trap that our existing dashboards don't reveal.
