---
layout: post
title:  "All Hands and AfterEffects"
date:   2018-06-23 09:00:00 +0530

---

This is my report for the [All Hands](https://wiki.mozilla.org/All_Hands/SanFrancisco2018) "Work Week" and one after. It was an absolute wonderful experience flying to the US for the first time. Everything from stay to movie nights, was taken care of the guys at Mozilla. I must say I worked more than I'd hoped to in [SanFrisco](https://thebolditalic.com/don-t-call-it-frisco-the-history-of-san-francisco-s-nicknames-the-bold-italic-san-francisco-5c14348d49c) (yeah thats what they call their city, or SF or just Frisco) thanks to the frequent lobby sittings and Coding sprints we had during the week. Coding together can be quite productive, contrary to what I used to think.

## My time in San Francisco, CA

It was only one week, but truly amazing. Right from meeting so many people I had worked with, to going out together, various discussions on Taskcluster to hacking together with [Jonas](https://mozillians.org/en-US/u/jonasfj/), [Dustin](https://mozillians.org/en-US/u/dustin/) and the team. All Hands is the best part of doing a GSoC with Mozilla. I along with two other volunteers biked to the [Golden Gate Bridge](https://en.wikipedia.org/wiki/Golden_Gate_Bridge) and took the ferry back. 

![sf_allhands0]({{site.url}}/assets/sf_allhands_0.jpg){: .center-image}

This is the team pic we clicked at the Conservatory of Flowers
![sf_allhands1]({{site.url}}/assets/sf_allhands_1.jpg){: .center-image}

Towards the end, we got to know that Jonas was leaving the company (he got an offer from a tech company which started as a PhD project). [Brian](https://mozillians.org/en-US/u/bstack/) will now be my mentor for the rest of the project. In the next video conference, we had to get him up to speed with what had been done and what was next so that I could complete my project on time. 


## Work on src/

I completed the function for input validation to make sure we do not send badly formatted requests to the Pulse server, and all cases of bad input are resolved by events itself. 

Another problem I discovered was that the [`EventSource`](https://www.w3.org/TR/2009/WD-eventsource-20090421/#eventsource) client tries to reconnect automatically everytime the connection is closed by the server. We want to avoid such unnecessary requests. It also sets a `Last-Event-Id` header which we can exploit. So every event had id `-`, and on every new connection if the `Last-Event-Id` header is set to `-` , we report an error and close the connection, thus saving resources.

## Work on testing

The biggest outcome of these two weeks was I had a somewhat stable [`mocha`](https://www.npmjs.com/package/mocha) testing framework. A significant part of the latter week was spent in making sure making the code more reusable so that new tests do not have a lot of copied code. I no longer have to run the server on one bash instance and my client on another. Also wrote a few tests for checking input validation, invalid exchanges and successful connections.

## What's Ahead

The next task is to get this PR merged. Then I will start working on the client implementation that is [pulse-inspector](tools.taskcluster.net/pulse-inspector) in ['tc-tools'](http://github.com/taskcluster/taskcluster-tools). That will then leave me with getting this deployed to `events.taskcluster.net`. Till then...

