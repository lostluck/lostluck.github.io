---
title: "My Hobby Beam Go SDK"
date:  2024-10-20T21:57:18-07:00
tags:
 - generics 
 - go
 - beam
categories:
 - Dev
 - Talks
 - Hobby SDK
---

I just made github.com/lostluck/beam-go public, having migrated the code out of
my experimental directory.

I'm going to write about this a lot, but I have come to the conclusion I can't
write about it all at once. So it's going to come in little bits and bursts.
In particularly motivations, and different design decisions that don't have
a great place in the documentation as it is.

And then I can do fun things, like have a conversation with a friendly question
asker.

# Why do a Beam SDK for a hobby?

Well, sometimes I can't work on things that scratch the right itch for my day
job. In this case in solving certain problems for the Go SDK itself.
If you aren't trying things out, you won't be adding those attempts to your
toolchest for other situations in the future.

Ultimately, it started because I wanted to play more with [Go Generics](https://go.dev/doc/tutorial/generics)
in the context of data processing in Go. In particular, could we make a better
SDK with them than the current version?

Also, when I started this, I was already making the [Beam Prism runner](https://beam.apache.org/documentation/runners/prism),
not only to solve problems, but also to learn how to make a Beam runner, and
better understand the Beam model itself. Developing a new SDK felt natural for it.

# Don't you do this for your day job?

Yes and no. I do work on improving the main Beam Go SDK for work, but there are
some things that are more important to work on sometimes.

You could argue that authoring a new SDK is in scope for some work I have about
writing an opinionated guide on how to write a Beam SDK. Right now, it's too
hard to start one, and unless you're already an expert on Beam, you won't know
what mistakes not to make.  In that sense, I'm making mistakes so future folks
don't have to.

Right now the best improvement to the Go SDK is making it able to run locally.
We're doing that with the Prism runner, and extending that work to other SDKs,
for a more consistent Beam experience. Fast, Local, iterative development is so
useful you know?

# Any other reasons?

I wanted to gain experience with try out other features of Go, like handling
vanity URLs, running a github project, managing github actions, and being otherwise
a good citizen of the Go package ecosystem. All those are transferable skills,
while also being useful for work, without being work.

The hobby SDK's documentation and package lives at `lostluck.dev/beam-go`, which
I could neatly handle within this Github Pages based website. It was important
to me that https://lostluck.dev/beam-go would go to the GoDoc.
(Nate Finch's [old guide for Go Package vanity URLs with hugo](https://npf.io/2016/10/vanity-imports-with-hugo/) still works nicely!)

You can read the docs all day long, but nothing beats trying it out yourself to
figure out how it works.

# That's Really All? 

...

Honestly? The current Go SDK has some issues, largely due to when it was designed
with respect to Go at the time.  But some things can't be fixed without making
breaking changes, or doing a tremendous amount of work to rewrite things without
doing that, or adding significantly more code to maintain.

I go into that a little bit with my talk at Beam Summit 2024.
[Beam SDKs don't have to look the same](https://beamsummit.org/sessions/2024/beam-sdks-don-t-have-to-look-the-same/).
Go there for a summary, slides, and the video!

Ultimately, by having it separate from mainline Beam, the expectations are lower
and there's more freedom to experiment with it.

# So how did this hobby SDK start?

I wanted to see what an alternative Beam Go could look like, and see how hard it
would be to make generic KV's (Key Value Pairs) in Go. There's no type compile time
erasure, covariance, or contravariance so how do you make sure you can encode or
decode things.

I wanted to answer questions like "Do we need to have lifecycle methodes like that?",
"Can we get type safe pipeline construction?", "Can coders be less annoying?", 
"Would generics reduce per element overhead?", "Can we avoid pre-registering DoFns and types", etc.
They go on.

One thing lead to another, and more questions were asked, and I needed to write
more code to get to the point I could answer them.

# What's next?

Next is that I'm going on a long vacation. Maybe I'll work on this a bit, maybe not.

Maybe I'll write about the SDK a bit during the vacation, maybe not.

What's for certain though is that you can look at the code, and if you see potential
beyond the current obvious faults, maybe provide some feedback?

No idea how I'll collect that for the moment. Maybe I'll make a feedback form.
In the meantime, reach out to me on [Bluesky](https://bsky.app/profile/lostluck.dev).

I only have the wordcount example for now. Most things reduce to wordcount TBH.

https://github.com/lostluck/beam-go/blob/main/examples/wordcount/wordcount.go#L40

The IOs are going to change for sure, among other breaking changes. Beam SDK's
don't need to be that big, but there is a lot involved in making one, but not
nearly as much as the Beam repo makes it out to be.
