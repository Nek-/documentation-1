---
outputFileName: index.html
sinceVersion: 3.0.0
---

# Optional HTTP Headers: EventID

When writing to a stream and not using the `application/vnd.eventstore.events+json/+xml` media type it is necessary that you specify an event ID with the event that you are posting. This is not required with the custom media type as it is also specified within the format itself (there is `EventId` on each entry in the format). `EventId` is used for idempotency within Event Store.

You can include an event ID on an event by specifying this header.

### [Request](#tab/tabid-1)

```bash
curl -i -d @event.json "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json" -H "ES-EventType: SomeEvent" -H "ES-EventId: C322E299-CB73-4B47-97C5-5054F920746E"
```

### [Response](#tab/tabid-2)

```http
HTTP/1.1 201 Created
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Location: http://127.0.0.1:2113/streams/newstream/1
Content-Type: text/plain; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Mon, 21 Apr 2014 21:43:03 GMT
Content-Length: 0
Keep-Alive: timeout=15,max=100
```

***

If you do not put an `ES-EventId` header on an append where the body is considered the actual event (e.g. not using `application/vnd.eventstore.events+json/+xml`) the server will generate a unique identifier for you and redirect you to an idempotent URI where you can post your event. It is generally recommended if you can create a UUID that you should not use this facility but it is useful when you are in an environment that cannot create a UUID.

### [Request](#tab/tabid-3)

```bash
curl -i -d @event.json "http://127.0.0.1:2113/streams/newstream" -H "Content-Type:application/json" -H "ES-EventType: SomeEvent"
```

### [Response](#tab/tabid-4)

```http
HTTP/1.1 301 FOUND
Access-Control-Allow-Methods: POST, DELETE, GET, OPTIONS
Access-Control-Allow-Headers: Content-Type, X-Requested-With, X-PINGOTHER, Authorization, ES-LongPoll
Access-Control-Allow-Origin: *
Access-Control-Expose-Headers: Location, ES-Position
Location: http://127.0.0.1:2113/streams/newstream/incoming/0a61c093-2fe5-4f9f-8c4e-8589099e917
Content-Type: ; charset=utf-8
Server: Mono-HTTPAPI/1.0
Date: Mon, 21 Apr 2014 21:47:46 GMT
Content-Length: 28
Keep-Alive: timeout=15,max=100
```

***

As you can see the server returned a '301 redirect' with a location header that points to <http://127.0.0.1:2113/streams/newstream/incoming/0a61c093-2fe5-4f9f-8c4e-8589099e917c> this generated URI is idempotent for purposes of retrying.
