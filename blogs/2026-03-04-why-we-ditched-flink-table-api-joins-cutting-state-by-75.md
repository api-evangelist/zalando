---
title: "Why We Ditched Flink Table API Joins: Cutting State by 75% with DataStream Unions"
url: "https://engineering.zalando.com/posts/2026/03/why-we-ditched-flink-table-api-joins-cutting-state.html"
date: "2026-03-04"
author: "Maryna Kryvko"
feed_url: "https://engineering.zalando.com/atom.xml"
---
The beauty of a high-level abstraction is that it lets you focus on the "what" rather than the "how." In the world of Apache Flink, the Table API is a powerful tool that abstracts away the complexities of stream processing, allowing developers to write SQL-like queries on streaming data. However, as we discovered in our journey with Flink, there are scenarios where the Table API's abstraction can be too heavy.
