---
layout: post
title: "Steam: The Curious Case of Connect"
---

So you've worked on your Steam SDK powered multiplayer game for a while now, and you're thinking... wouldn't it be nice to support connecting to servers using the `steam://connect/...` browser protocol? Maybe even having support for listing and connecting to servers through the official Steam server browser? That cannot be so difficult, right? 

Wrong! Turns out that it is in fact an undocumented rabbit hole with a lot of gotchas and conflicting information. Let me walk you through my journey, which will also reveal the solution to the problems encountered.

I started out my experiment by simply trying to run `steam://connect/127.0.0.1:5038`, to see what would happen, and was immediately greeted with this error dialog. Fascinating, I thought to myself. Why would the app id provided by the server be invalid? My game server is using SteamGameServer through the Steam SDK, and I had assumed that this would handle all of this for me. But there were a bunch of questions, none of which the documentation helped me answer. 

Before we move on, let's take a step back and look at the SteamGameServer API and how I was using it.

Looking at the documentation, it's not entirely clear *what* SteamGameServer even is. On one hand, it's a bunch of APIs that seem to be used to authenticate with Steam and enable sending game server telemetry to the Steam master servers, but on the other hand it's also an alternative entry point to *some* of the Steam APIs. You don't want to require the server to be running the Steam client, which is a hard requirement for the usual APIs. For example, if you attempted to use `SteamUtils()` on the server, it would complain about the API not being initialized. However, attempting to remedy this by running `SteamAPI_InitEx()` would quickly reveal that this requires the Steam client running.

The solution is the "SteamGameServer" prefixed versions of some of these APIs. For example, there's a `SteamGameServerUtils()` function which is the GameServer equivalent, and which only requires `SteamGameServer_InitEx()` in order to resolve.

Then, as mentioned, there is the actual "SteamGameServer" API, accessible through the `SteamGameServer()` function, and which seems to provide the authentication and telemetry reporting. This design is already confusing enough, but it gets more confusing.

