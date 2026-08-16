---
title: "We turned off Pub/Sub and nobody noticed"
url: "https://incident.io/blog/we-turned-off-pub-sub-and-nobody-noticed"
date: "2026-08-11"
feed_url: "https://incident.io/blog.xml"
---
Our entire event-driven platform ran through a single message broker, which made it a single point of failure. So we added a second one. This is the story of building an event load balancer, the queuing theory behind it, and the final chaos test where we turned off Pub/Sub in production and nobody noticed.
