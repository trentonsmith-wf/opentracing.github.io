---
layout: post
title: "weekly hangout: DAG tracing, span errors, basictracer-py"
---

Dimitris (@lookfwd) joined us from London at 12:45am local time (!!!) to talk about [opentracing.github.io/issues/85](https://github.com/opentracing/opentracing.github.io/issues/85). Thanks!

Rough notes (not in chronological order, and not complete):

- @lookfwd wants spans to record in the underlying tracing system even before they've been finished. Everyone agreed that this was a reasonable thing to want in certain scenarios, though there is still the question of whether OT should have explicit methods to force per-span `flush()` or similar. For now we are going to leave this sort of detail purely in the API implementation.
- @lookfwd also wants to record the causal relationship between spans that do not overlap in time (e.g., perhaps a user request returns as soon as a single replica returns success, but as a developer it's also important to trace the second/third/etc replica write success). Those on the hangout agreed that this was something OT should be able to represent. When we inject a span into a carrier, the carrier contents can be used as a "pointer" value from some other span... but where to record those pointers? `join`? `setTag`? Something else? People will make proposals on [opentracing.github.io/issues/85](https://github.com/opentracing/opentracing.github.io/issues/85).
- @bg451: basictracer getting close to being ready in python!
- span errors: we need to decide on how to represent them
  - could add a span method, use some standard span tag, or infer from a log record in an error state
  - TL;DR: we will formalize a tag name+value (`"error": true`, most likely) for span errors
- cloud sleuth: @bensigelman to talk with Marcin about future plans
