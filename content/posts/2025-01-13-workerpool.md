---
title: "Beam External Worker Pool"
date: 2025-01-13T22:18:46-08:00
tags:
- beam
- golang
- dev
- projects
categories:
- Dev
---

Today I merged a PR to add [the ability to run a Worker Pool](https://github.com/apache/beam/pull/33572)
for the Beam Go SDK. I'm writing this quite late, so it's more of a sketch of a draft of
documentation than real documentation. So this will be light on links and examples or diagrams for now.

It's not something one always needs to use, but it adds flexibility to the SDK. 

<!--more-->

Apache Beam's Portability framework as encoded by the Protocol Buffers has something
called the ExternalWorkerPoolService.

Pre dominantly used to support "loopback mode" when executing pipelines, it's
a general approach to allow a Beam runner to spin up workers on demand.

But I think I'm getting ahead of myself.

When authoring any code, it's very useful to be able to iterate on the program
locally. The faster you can try things out, and find and eliminate bugs, the 
better the code will be, and the more fun the experience tends to be.
The same is true for your data processing pipelines.

### Local Execution with Loopback Mode

Beam enables this by what it calls "loopback mode". By setting your job's
environment type to `LOOPBACK`, the framework allows your job to be executed
locally. This lets you use the debugger, set breakpoints, and avoid most of the
networking and distributed bits that make things hard.

The Loopback environment type tells the framework to host an in process instance
 of the ExternaWorkerPoolService, and uses it's network endpoint to replace the normal
execution environment.

The Runner then connects to that address, and asks for it to "start" a worker,
giving it all the usual Beam Portability information to it. That worker then
goes on to get work and execute it.

You gain all the speed of avoiding additional layers like docker, at the cost
of not being able to distribute the work. As a result, Loopback mode isn't really
the fastest way to execute all jobs. You're limited to what your local machine
can have in memory and process in parallel afterall.

### External Worker Pool, as a Service

But what if you didn't want to run your workers on *your* machine? There's no
reason the ExternalWorkerPoolService has to be in the same process as the
launching program.

The service can be hosted anywhere, as long as the runner can connect to it.
The implementation just needs to produce a worker, that will connect to the
provided Beam FnAPI endpoints, and execute the given pipeline. This includes
the various artifacts and such to run it.

The feature I added, is to be able to spin up a stand alone use of the service
and do exactly that. All built into the standard Beam Go SDK Container.

The root of it is that you can start the container with a `--worker_pool` flag
and tunnel out port 50000 from the container. This makes the SDK container
boot loader program start the workerpool version of the service, instead of
starting as an SDK worker itself.

The trick in the implementation is that it starts a separate process when
StartWorker is called, executing the bootloader itself when it does so.

That new bootloader process then performs the normal Beam Container Contract,
where it connects to the provisioning service to get the different endpoints, 
gets the artifacts from from the artifact service, starts it's own worker binary
instance, which ultimately, connects back to the runner's control service to
get work.

### OK, but what is it for?

The best use is when a runner is unable to start up it's own workers for a given
SDK. This lets you preconfigure a side car service for the work that needs to
expand out and do the work.

For example, this is needed when executing a pipeline on a runner on Kubernetes.
You'd sometimes like to be able to bound the number of machines or pods that can
serve as workers within your cluster. This lets you arrange a dedicated set of
pods that can behave as workers for arbitrary pipelines. 

The main downside is that it needs to be manually pre-configured. Sorry about that.

But with this as a building block, and a few more changes coming down the line
we may be able to build something more self directing in the future.