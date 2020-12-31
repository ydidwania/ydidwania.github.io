---
layout: post
title:  "GSoC Selection"
date:   2018-05-01 13:45:53 +0530
comments: true

---
![gsoc]({{site.url}}/assets/gsoc_cover.png "Google Summer of Code"){: .center-image}

This semester finally ended with some great news and that is reason enough to write my first blogpost. My proposal for [Google Summer of Code 2018][gsoc] with Mozilla has been accepted. Here I will write about how I started contributing to Mozilla and open source in general and then a bit on my project and what's ahead for the summers.

## A Little Prep 

The title of my project is **Improved Taskcluster Pulse Backend**. I will be working within [Taskcluster](https://docs.taskcluster.net/) team to
rewrite tascluster-events. [Pulse][pulse] is a system to exchange messages, giving more visibility to Mozilla's tools and allowing for more dynamic and informative tools. It follows a publisher-subscriber pattern whereby a publisher can send messages to a topic exchange and consumers can create queues to bind to thses exchanges. 

Each message is published with a routing key. The structure is something similar to different threads in a subreddit. Subscribers can specify which exhange and route they want to subscribe to. All if this is managed by a [RabbitMQ](https://www.rabbitmq.com/) server at `pulse.mozilla.org`



## What will I be doing?

Communication becomes more and more important as the project becomes larger. The web-client, [Pulse Inspector](https://tools.taskcluster.net/pulse-inspector) is a tool to verify that messages being published are being handled correctly to their specified route, and they end up in receiver queues.

Coming back to taskcluster-events. It is a service for web clients to listen to pulse messages. **RabbitMQ uses [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)** and connections from browsers are usually [tcp](https://en.wikipedia.org/wiki/Transmission_Control_Protocol), making it harder for web clients to directly use RabbitMQ.
Tascluster-events solves the problem by using Websockets, and creating RabbitMQ queue for each Websocket connection and pushes messages from the queue to the websocket. It is old and poorly designed. My job will be to completely rebuild taskcluster-events from scratch, ensuring it is robust and free of major bugs or failures.

I am fortunate to have **Jonas Finnemann Jensen** (irc: [jonasfj](https://mozillians.org/en-US/u/jonasfj/)) as my mentor for this project. Lots to learn from him. The finer details about the project can be found in my proposal [here][proposal]

The RFC for this is [here][rfc]

## Plan for Ahead

Well, vacations always start with a lot of plans in mind. I will have to go over my proposal again, owing to the recently concluded end semester exams. I aim to be proficient in javascript by the end of it. Coding phase begins next week, but I want to have my environment and repository ready before that. This blog will primarily be used to document any progress I make on my project, making it easier to revisit in the future. Looking forward to an exciting summer and ahead !!




[gsoc]: https://summerofcode.withgoogle.com/
[proposal]:   https://drive.google.com/file/d/1egLVTK9WHlgGaYQfeFiUHMSAnAoLjFTF/view?usp=sharing
[rfc]: https://github.com/taskcluster/taskcluster-rfcs/pull/104
[pulse]:https://wiki.mozilla.org/Auto-tools/Projects/Pulse

