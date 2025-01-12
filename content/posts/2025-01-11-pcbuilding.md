---
title: "PC Building Notes"
date: 2025-01-11T15:09:42-08:00
tags:
- nas
- dev
- pc
categories: 
- Personal
---

So I'm building a NAS with [these parts](https://pcpartpicker.com/list/LBM74p)
While I'm still missing the motherboard and the hard drives, I am able to start
building the NAS without them.

As it turns out, there were a few more bits of due diligence I 
probably should have done before making my order. Oh well.
I'm writing them down to help someone in the future (me).
 
<!--more-->

Mostly I'm able to install the PSU and start planning the cable routing for power
and other features. I've never built in an ITX case before, so everything is
quite compact. Things like how the PSU is positioned and installed were
unfamiliar to me.

As I was doing that, I became concerned about three things.

### Can it boot?

First was whether or not I'd be able to boot the computer and have any
display output without a GPU of any kind. My chosen CPU, the Ryzen 7 3700X doesn't
have any integrated graphics, and I personally don't have a GPU to spare. 
Apparently that means it's very unlikely I'll be able to boot to BIOS at all,
let alone to any other interface.

My options are either, find a different CPU with integrated graphics, and fits
into the slot, or find a GPU to use instead, that would fit into the case.

Getting a different CPU would sort of defeat the point of building this to try
and get some more use out of the CPU, since it was the only part I had originally.
But the NAS was never really intended for having a GPU, so it's very up in the
air which path I go down. 

Some friends have offered some of their old GPUs, but none make the 250mm 2 slot
length requirement that the case has. Though one has offered an old office grade
Quadro card... that might be exactly what this build needs for now, until I need
a "real" GPU in there. Assuming they're willing to part with it of course.

I'm not currently in a rush to get a new GPU for a box that I'm not gaming on.

At the very least, the PSU has plenty of power for one, when it gets there.

### Do the drives work with the NAS' backplane?

The JONSBO N3 case has space for 8 hot swappable drives. When I was routing power
for that, I noticed that the connectors for the drives were unfamiliar to me.

So I started looking it up.

I learned that SATA drives are built to connect to SAS connections, but not the
reverse. So, it seems that I'm good there.

### SATA Cable routing

I forgot entirely that I might need to get cables to safely route them for this
build. The motherboard has the SATA ports coming off the "front" of the board,
but at a 90 degree angle. I think that might be a bit crowded to start, even
with some 90 degree connectors. The power cables are also coming into that space
too.

These can wait until I have the motherboard at least. Then I can see what I need
and get the shortest cables that comfortably fit for routing the SATA cables.
I'm expecting to at least need 4 "right" angle cables, and two "left" angle
cables, in order to make use of the ports, for the drives I'm getting.

### How I'm feeling about it

I'm still feeling pretty good about the build. The "early" GPU was unexpected
though, I certainly don't need anything fancy in there.

Like, ideally, I'd be able to salvage most of these parts from my current build
as they're being replaced, and build a new computer with them. They're still
good parts. My current 2070 Super GPU not fitting certainly puts a damper on that
plan a little. In hindsight, I should have made sure whatever NAS case I was getting
would fit the thing. As it stands, it's 7mm too long, so unless I'm willing
to do some case surgery, it won't fit. 

I'll need to find something else to do with if I succeed at getting a 5080 FE
when they launch at the end of the month.
