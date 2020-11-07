# Day 1

## Goal

Basically, the goal today is to get aquainted with the terminology in HTTP/1.1 and have an introduction to the HTTP/1.1 protocol.

## Server and other definitions in HTTP/1.1

>An application program that accepts connection in order to service requests by sending back responses. Any given program may be capable of being both a client and a server; our use of these terms refers only to the role being performed by the program for a particular connection, rather than to the program's capabilities in general. Likewise, any server may act as an origin server, proxy, gateway, or tunnel, switching behavior based on the nature of each request.

hmm.. so I basically need to focus on the sole role of servicing requests and sending back responses. and for the time being focus on building an origin server, which, based on the rfc, means:

>The server on which a given resource resides or is to be created.

And nothing more... Actually I'll be surprised if I implemented a working origin server LOL.

hmm cache ?
I've heard that before and think I know what it means... the official definition is to be kept here for future reference.
>A program's local store of response messages and the subsystem that controls its message storage, retirieval, and deletion. A cache stores cacheable responses in order to reduce the response time and network bandwidth consumption on future, equivalent requests. Any client or server may include a cache, though a cache cannot be used by a server that is acting as a tunnel.

>A response is cacheable if a cache is allowed to store a copy of the response message for use in answering subsequent requests.the rule for determining the cacheability of HTTP responses are defined in section 13. Even if a resource is cacheable, there may be additional constraints on whether a cache can use the cached copy for a particular request.

Now let's forget about cache.

- **first-hand** response comes directly from an origin server or it's validity has been checked directly with the origin server

- **age** of a response: time since it was sent or validated

- **freshness lifetime** : time between generation of response and its expiration time

- **fresh** request: age <= freshness lifetime

- **stale** !fresh

- **upstream/downstream** : describe flow of messages - upstream -> downstream

- **inbound/outbound** : request and response paths
    inbound : traveling towards the origin server

    outbound : towards the user agent.

## Overall Operation

A client sends a request to a server.

Request = request method, URI, protocol version, MIME-like msg containing request modifiers, client info and a possible body content.

Server responds.

Response = statis line protocol version and success/error code, MIME-like msg with server info, entity metainfo and a possible entity-body content.

I am going to focus on the simple situation where a user agent initiates the communication by send a request to an origin server via single connection.

>request chain ------------------------> 
UA -------------------v------------------- O 
<----------------------- response chain 

UA: user agent
v: connection
O: origin server


HTTP communication usually takes place over TCP/IP. With a default port "TCP 80" (doesn't matter tho).

HTTP only presumes a reliable transport. Meaning that any protocol that provides such guarantees can be used

>In HTTP/1.0, most implementations used a new connection for each request/response exchange. In HTTP/1.1, a connection may be used for one or more request/response exchanges,although connections may be closed for a variety of reasons (see section 8.1).
