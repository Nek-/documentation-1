---
title: "Optional HTTP Headers: LongPoll"
section: "HTTP API"
version: "4.0.2"
exclude_from_sidebar: true
---

You use the `ES-LongPoll` header to instruct the server that when on the head link of a stream and no data is available the server should wait a period of time to see if data becomes available.

<span class="note--warning">
Version 2.1 and higher support the `ES-LongPoll` header. Lower versions will ignore the header.


You can use this to provide lower latency for Atom clients instead of client initiated polling. Instead of the client polling every say 5 seconds to get data from the feed the client would send up a request with `ES-LongPoll: 15`. This instructs the backend to wait for up to 15 seconds before returning with no result. The latency is therefore lowered from the poll interval to about 10ms from the time an event is written until the time the HTTP connection is notified.

You can see the use of the `ES-LongPoll` header in the following cURL command.

First go to the head of the stream (in this case we are using the default chat of the chat sample).

<div class="codetabs" markdown="1">
<div data-lang="request" markdown="1">
```bash
curl -i http://127.0.0.1:2113/streams/chat-GeneralChat -H "Accept: application/json"
```
</div>
<div data-lang="response" markdown="1">
```http
HTTP/1.1 200 OK
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Cache-Control: max-age=0, no-cache, must-revalidate
Vary: Accept
ETag: "0;-43840953"
Content-Type: application/json; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Thu, 08 May 2014 10:12:27 GMT
Content-Length: 1348
Keep-Alive: timeout=15,max=100

{
  "title": "Event stream 'chat-GeneralChat'",
  "id": "http://127.0.0.1:2113/streams/chat-GeneralChat",
  "updated": "2014-05-08T07:10:23.666463Z",
  "streamId": "chat-GeneralChat",
  "author": {
    "name": "EventStore"
  },
  "headOfStream": true,
  "selfUrl": "http://127.0.0.1:2113/streams/chat-GeneralChat",
  "eTag": "0;248368668",
  "links": [
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat",
      "relation": "self"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/head/backward/20",
      "relation": "first"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/1/forward/20",
      "relation": "previous"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/metadata",
      "relation": "metadata"
    }
  ],
  "entries": [
    {
      "title": "0@chat-GeneralChat",
      "id": "http://127.0.0.1:2113/streams/chat-GeneralChat/0",
      "updated": "2014-05-08T07:10:23.666463Z",
      "author": {
        "name": "EventStore"
      },
      "summary": "UserJoinedChat",
      "links": [
        {
          "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/0",
          "relation": "edit"
        },
        {
          "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/0",
          "relation": "alternate"
        }
      ]
    }
  ]
}
```
</div>
</div>

Then grab the previous `rel` link `http://127.0.0.1:2113/streams/chat-GeneralChat/1/forward/20` and try it. It returns an empty feed.

<div class="codetabs" markdown="1">
<div data-lang="request" markdown="1">
```bash
curl -i http://127.0.0.1:2113/streams/chat-GeneralChat/1/forward/20 -H "Accept: application/json"
```
</div>
<div data-lang="response" markdown="1">
```http
HTTP/1.1 200 OK
Access-Control-Allow-Methods: GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Cache-Control: max-age=0, no-cache, must-revalidate
Vary: Accept
ETag: "0;-43840953"
Content-Type: application/json; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Thu, 08 May 2014 10:13:58 GMT
Content-Length: 843
Keep-Alive: timeout=15,max=100

{
  "title": "Event stream 'chat-GeneralChat'",
  "id": "http://127.0.0.1:2113/streams/chat-GeneralChat",
  "updated": "0001-01-01T00:00:00Z",
  "streamId": "chat-GeneralChat",
  "author": {
    "name": "EventStore"
  },
  "headOfStream": false,
  "links": [
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat",
      "relation": "self"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/head/backward/20",
      "relation": "first"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/0/forward/20",
      "relation": "last"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/0/backward/20",
      "relation": "next"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/chat-GeneralChat/metadata",
      "relation": "metadata"
    }
  ],
  "entries": []
}
```
</div>
</div>

The entries section is empty (there is no further data that can be provided). Now try that URI with a long poll header.

```bash
curl -i http://127.0.0.1:2113/streams/chat-GeneralChat/1/forward/20 -H "Accept: application/json" -H "ES-LongPoll: 10"
```

If you do not insert any events into the stream while this is running it will take 10 seconds for the HTTP request to finish. If you append an event to the stream while its running you will see the result for that request when the event is appended.
