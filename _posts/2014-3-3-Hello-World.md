---
layout: post
title: "Steam: The Curious Case of Connect"
---

So you've worked on your Steam SDK powered multiplayer game for a while now, and you're thinking... wouldn't it be nice to support connecting to servers using the `steam://connect/...` browser protocol? Maybe even having support for listing and connecting to servers through the official Steam server browser? That cannot be so difficult, right? 

Wrong! Turns out that it is in fact an undocumented rabbit hole with a lot of gotchas and conflicting information. Let me walk you through my journey, which will also reveal the solution to the problems encountered.
