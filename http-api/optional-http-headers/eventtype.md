---
title: "Optional HTTP Headers: EventType"
section: "HTTP API"
version: "4.0.2"
exclude_from_sidebar: true
---

> [!NOTE]
> 
This event is only available in version 3.0.0 or higher of Event Store.


When writing to a stream and not using the `application/vnd.eventstore.events+json/+xml` media type it is necessary that you specify an event type with the event that you are posting. This is not required with the custom media type as it is also specified within the format itself.

You can use the `ES-EventType` header as follows.

<div class="codetabs" markdown="1">
<div data-lang="request" markdown="1">
```bash
curl -i -d @event.json "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json" -H "ES-EventType: SomeEvent" -H "ES-EventId: C322E299-CB73-4B47-97C5-5054F920746E"
```
</div>
<div data-lang="response" markdown="1">
```http
HTTP/1.1 201 Created
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Location: http://127.0.0.1:2113/streams/newstream/1
Content-Type: text/plain; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Mon, 21 Apr 2014 21:26:48 GMT
Content-Length: 0
Keep-Alive: timeout=15,max=100
```
</div>
</div>

If you now view the event in the UI or through cURL it will have the `EventType` of `SomeEvent` associated with it.

<div class="codetabs" markdown="1">
<div data-lang="request" markdown="1">
```bash
curl http://127.0.0.1:2113/streams/newstream/1
```
</div>
<div data-lang="response" markdown="1">
```http
HTTP/1.1 200 OK
Access-Control-Allow-Methods: GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Cache-Control: max-age=31536000, public
Vary: Accept
Content-Type: application/vnd.eventstore.atom+json; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Mon, 21 Apr 2014 21:29:27 GMT
Content-Length: 585
Keep-Alive: timeout=15,max=100

{
  "title": "1@newstream",
  "id": "http://127.0.0.1:2113/streams/newstream/1",
  "updated": "2014-04-21T18:26:48.731192Z",
  "author": {
    "name": "EventStore"
  },
  "summary": "SomeEvent",
  "content": {
    "eventStreamId": "newstream",
    "eventNumber": 1,
    "eventType": "SomeEvent",
    "data": {
      "something": "has data"
    },
    "metadata": ""
  },
  "links": [
    {
      "uri": "http://127.0.0.1:2113/streams/newstream/1",
      "relation": "edit"
    },
    {
      "uri": "http://127.0.0.1:2113/streams/newstream/1",
      "relation": "alternate"
    }
  ]
}
```
</div>
</div>
