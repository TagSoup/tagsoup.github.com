---
layout: post
title: "Polling is for chumps - node.js with EventSource"
date: 2012-02-22 00:23
comments: true
categories: [nodejs, level-up, eventsource]
author: Chris Lee
---
You know what sucks? Making hundreds of round trips to a server to see if there's any new data you can grab.

But modern browsers (well, Firefox and Chrome and Safari) support a much neater option. With the right `Content-Type` header, some special sauce in the client, and a modern browser platform, you can use the HTML5 [EventSource API][0] to establish a single long-term connection for each client, and the server pushes new data whenever necessary.

Let's start with a look at the server-side implementation.

{% gist 1883462 app-server.js %}

Things to note: The secret sauce here is all in the `/events` handler. It's pretty simple - we watch a log file to see when its size changes, and when that happens, we read the new data from the end of the file and send it to the client tagged with the event name `data` (so if you have multiple kinds of events, you can interleave different messages by tagging them with different `event` names and the client will be able to properly distinguish them). And of course, if the client closes the connection, we want to free up the open file descriptor and the watch on the logfile.

Now, how do we consume this data from the client?

{% gist 1883462 app-client.js %}

It's so simple it almost doesn't need an explanation. The important things to note are: of course the URL has to point at the same endpoint that the server is exporting the stream on, and we add a listener for the `data` event which we know the messages from the server will be tagged with. If you want to support multiple message types, you just add another listener on the client for each different `event` name and you're good to go.

[0]: http://dev.w3.org/html5/eventsource/
