---
title: "Go Without Generics"
date: 2020-10-02T10:19:19-07:00
tags:
 - generics 
 - go
 - gowestconf
 - conference
categories:
 - Dev
 - Talks
---

Today I gave a talk on Generics in Go, or rather, about the kinds of hoops we need
to jump through to do something in Go when we don't have Generics. In my case it's
invoking arbitrary functions together in a pipeline, like one needs to do for
Apache Beam.

This post is a placeholder for an article version of the talk, but for now, here are
the relevant links.

<!--more-->

GoWestConf link for talk: https://www.gowestconf.com/team/go-without-generics%2C-a-retrospective 

Cur Draft Design (2020-09): https://go.googlesource.com/proposal/+/refs/heads/master/design/go2draft-type-parameters.md 

The Next Step for Generics: https://blog.golang.org/generics-next-step 

Go2Playground: https://go2goplay.golang.org/

Annotated Code for this talk:
https://github.com/lostluck/experimental/tree/master/talks/retrospective 

Type Parameter Wrappers Generic Go: https://go2goplay.golang.org/p/5W39ybOQooA 

Code Generation in Apache Beam:
https://github.com/apache/beam/blob/master/sdks/go/pkg/beam/core/util/reflectx/call.go
https://github.com/apache/beam/blob/master/sdks/go/pkg/beam/util/starcgenx/starcgenx.go 
https://github.com/apache/beam/blob/master/sdks/go/pkg/beam/util/shimx/generate.go 

Slides: https://docs.google.com/document/d/1-N-0QNXbTnbetbdT1Ljwjl1pm5D0YtEZvcqHnygnYqQ/edit
Script: https://docs.google.com/presentation/d/1LreJX4MEpylZtpYxjlzWByU7wEPLv-Lii2Wo4rV_5vo/edit#slide=id.g9c729dc48f_0_127