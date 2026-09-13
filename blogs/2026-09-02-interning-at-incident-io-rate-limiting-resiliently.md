---
title: "Interning at incident.io: rate limiting, resiliently"
url: "https://incident.io/blog/interning-at-incident-io-rate-limiting-resiliently"
date: "2026-09-02"
feed_url: "https://incident.io/blog.xml"
---
Our rate limiter depends on Valkey. If Valkey goes down we fail open and stop limiting which isn't good enough for our platform. As an intern, I built per-pod in-memory top-k buffers so we keep rate limiting even with the backing store gone.
