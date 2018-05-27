---
layout: post
title:  "GSoC Week 1 and 2 : A New Beginning"
date:   2018-05-26 09:00:00 +0530

---

These have been quite eventful two weeks. I am writing two weeks together beacuse there isn't much to write about week one.I started watching Star Wars back then and was instantly hooked. I am already done with The Original and The Prequel trilogies, and probably ended up with a little guilt too :P. Now coming back to the important stuff.

![star-wars]({{site.url}}/assets/star-wars.jpg "Star Wars"){: .center-image}

## Major Changes

In the first chat I had with Jonas, post selection, he told me about this new technology, Server Sent Events, which can essentially replace Websockets(which we initially planned to use, and my proposal was based on it.) in a lot of places. The Mozilla Developer Network had advised him to use it. I will elaborate on what is SSE as we go on. A major advantage was that client side implementation becomes really simple. But this also means, we will have to write it again. Some browsers like the Microsoft Edge dont support it as yet, but there are [polyfills](https://github.com/Yaffle/EventSource) for that, so all good.

After much deliberation, we have decided to drop the automatic reconnect feature, because it allows us to be more  relaible and robust, without leaking resources. It took a while for me to understand the thought behind it, I am sure it will take another blogpost to do justice. This trims down my project even further, which means we can get to `taskcluster-github` or r14y bugs faster :)


## Server Sent Events

As the name suggests, it is speifically designed for the purpose of sending notifications / messagges from the server. The server maintains an event stream which is nothing more than a simple text stream over an http connection. It is essentially a one way channel flowing from server to client only, unlike Websockets where two way commmunication is possible. The downside is that the connection has to be kept open for as long as you want to receive messages. The docs here. Each message has four fields, `event`, `id`, `data`, `retry`, and two messages are separated by a newlline `\n`. All other fields are ignored by `text/event-stream`. So our event stream looks something like this -
```
event: pulse-ready\n
data: {message: "we are now actively listening"} \n
\n
event: pulse-message\n
data: {....} \n
id: <AMQP-message-id> \n
\n
event: pulse-message\n
data: {...} \n
id: <AMQP-message-id> \n
\n
```

The client side code becomes as simple as this  - 
```
var source = new EventSource('URL');

    // handle messages
    source.onmessage = function(event) {
        // Do something with the data:
        event.data;
    };
```


## Alternatives in the past

The most widely used alternative were Websockets. Historically, the way to receive information is to ask for it, and this is called Polling. A slight variant is Long Polling in which the server holds the request until it has new data to send. However it has several issues like high latency and overloading the server. I found the image below immmensely helpful and also the blogs [here](https://streamdata.io/blog/server-sent-events/) and [here](https://www.smashingmagazine.com/2018/02/sse-websockets-data-flow-http2/).

![sse]({{site.url}}/assets/sse.png "SS"){: .center-image}

## What have I done ?

The first week went by reading documentations and trying to get SSE working locally. Jonas advised me to take a look at taskcluster-queue and structure my code in a similar way. It is really well written and Jonas told me he'd done long polling in it, which is awesome. I started out with [this](https://github.com/taskcluster/taskcluster-events/pull/6) PR. It has only 13 commits as of now. I am using various `taskcluster-lib-*` libraries, which is the standard. 

There is a major update of all libraries going on to use `rootUrl`, as part of a bigger Redeploability initiative, to make Taskcluster easy to use for people both inside and outside of the Mozilla network. Here are the trackers - [Internal Redpeployability](https://bugzilla.mozilla.org/show_bug.cgi?id=1427839), [External Redeployability](https://bugzilla.mozilla.org/show_bug.cgi?id=1427838). The extensive list of r14y bugs are [here](https://bugzilla.mozilla.org/buglist.cgi?quicksearch=redeployability&list_id=14167797).



## How does it affect my project ?

I had originally written the code using the same versions as in `taskcluster-queue`. Jonas told me to update to v10 as all `tascluster-lib-*` had been bumped up to reflect major changes in specification. For example `taskcluster-lib-loader` went from v2.0.0 to v10.0.0 

I have since had to make changes, which has slowed down my work. Jonas came up with this idea that we should use `taskcluster-lib-api` to create an API endpoint which we can use (or in some ways abuse) for our needs. The client can send a GET request to this endpoint to establish a `keep-alive` connection. 

I am unable to a successful build at the moment. I will be coordinating with Dustin about the latest changes, which haven't yet made it to the docs. 

##  Coming up

First on my list is to get this thing running. After that, I will have to create a `docs/` folder to document the endpoint. Then, write a few simple tests and at the same time add PulseListener.

## Other news

I was lucky enough to have gotten my visa interview in such quick time. I finally received my US visa to be able to travel to San Francisco for All Hands. I will be leaving on June 11. I will have to complete the thing above if I am stay on schedule.

