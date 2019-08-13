---
layout: blog
comments: true
book: true
title:  "Play framework Akka WebSocket"
tags:
- Akka
- Scala
- Play framework
- WebSocket
background-image: https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg
date:   2019-08-13 11:43:00
category: book
---

## Play framework Akka WebSocket

### WebSocket

**WebSocket** is a computer communications protocol, providing full-duplex communication channels over a single TCP connection. The **WebSocket** protocol was standardized by the IETF as RFC 6455 in 2011, and the**WebSocket** API in Web IDL is being standardized by the W3C. **WebSocket** is distinct from HTTP.



#### Problem1: How to allocate single Actor for every websocket connection?

Use

```java
public static Props props(ActorRef out) {
	return Props.create(BundleActor.class, () ->new BundleActor(out));
}
```

Rather than

```java
public static Props props(ActorRef out) {
	return Props.create(BundleActor.class, out);
}
```



#### Problem2: How to keep websocket alive?

As websocket shuts down the connection after each 65 seconds without operations. If the connection should be kept, heartbeat packages are mandatory to keep the connection alive.

JavaScript:

```javascript
// send heartbeat package to keep the connection alive
function keepAlive() {
    console.log("send heartbeat pack.");
    var timeout = 20000;
    if (socket.readyState === WebSocket.OPEN) {
    	socket.send('');
    }
    timerId = setTimeout(keepAlive, timeout);
}

function cancelKeepAlive() {
    if (timerId) {
   		clearTimeout(timerId);
    }
}

socket.onopen = function (e) {
    // *other logic*
    keepAlive();
};

socket.onclose = function(event) {
    // *other logic*
    cancelKeepAlive();
}
```

Client:

```java
public void dispatcher(String query, BundleActor bundleActor) {
    if(query.equals("")) {
        System.out.println("Heartbeat detected.");
        return;
    }
}
```



### Reference

[1]. https://en.wikipedia.org/wiki/WebSocket

[2]. https://www.jstips.co/en/javascript/working-with-websocket-timeout/