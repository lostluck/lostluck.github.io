---
title: "Silly Project Ideas"
date: 2025-01-10T21:28:12-08:00
tags:
- dev
- go
- beam
- prism 
- hobby SDK
categories: 
- Dev
- Hobby SDK
---

Here are some weird project Ideas I'd love to try if I ever find the time. 

<!--more-->

* Make [Prism](https://beam.apache.org/documentation/runners/prism/) "self distributed and scaling" using [Cloud Run](https://cloud.google.com/run).
  * Basically the idea would be to host an instance on Cloud Run, serving it's JobManagement service.
  * But replace the local docker execution with Cloud Run launches for worker container launches, connecting back to the parent prism instance for work orchestration.
  * The main prism instance can't scale, but the UI should be hostable without too much problem.
  * Could add Tailscale for more fun.
  * Entirely a fun hack without also adding a cloud persistence layer too.
* Make [Prism] integrate with Kubernetes. Distributes work in the cluster.
  * Basically the same idea, but with Kubernetes operations instead. 
  * Same problems.
* Find the branch of the Go compiler that has the work for Memory/GC Regions, and have it store things into Cuda Unified Memory Instead to simplify GPU integration.
  * Totally wild endgame idea. Would need to actaully learn some Cuda and it's CGo bindings first.
  * In principle could be used to more naturally toggle GPU based work.
* Heck, even just do my normal distributed ray tracer, but batch the rendering stage onto a GPU.
* Try using opencv with BeamGo generally.
* Actually finish the Hobby SDK.
  * Stateful Transforms - State and Timers
  * On Window Expiration
  * Real Windowing
  * Streaming Splittable-DoFns
  * Leverage Go 1.24 Generic Aliases to make the API more convenient.
* And of course
 * Submit talk proposals to conferences.

So might to potentially do.

Never enough time. 

----

I think I'm not going to try too hard to post on Thursdays.
Between the Frelard Run Club, and Pub Night, it's quite a packed day.
