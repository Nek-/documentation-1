---
outputFileName: index.html
---

# Step 2 - Read events from a stream and subscribe to changes

[!include[<Getting Started Intro>](~/getting-started/_intro.md)]

This second step covers reading events from a stream and subscribing to changes to events in a stream.

## Read a stream of events

Event Store exposes all streams as [atom feeds](http://tools.ietf.org/html/rfc4287), and you can read data from the stream by navigating to the _head URI_ of the stream <http://127.0.0.1:2113/streams/<STREAM_ID>> with cURL, or click the _Stream Browser_ tab in the Admin UI and you see the stream you created in step 1.

![The Admin UI Dashboard](~/images/es-web-admin-stream-browser.png)

### [Request](#tab/tabid-6)

[!code-bash[getting-started-read-stream-request](~/code-examples/getting-started/read-stream.sh?start=1&end=1)]

> [!NOTE]
> This returns the feed in JSON format, you can also use `Accept:application/atom+xml` if you prefer XML.

### [Response](#tab/tabid-7)

[!code-http[getting-started-read-stream-response](~/code-examples/getting-started/read-stream.sh?range=3-)]

* * *

### [.NET Client API](#tab/tabid-dotnet-client)

To use the .NET API, use the following method passing the stream name, the start point in the stream, the number of events to read and whether to follow links to the event data:

[!code-csharp[getting-started-read-stream-request](../../EventStore.Samples.Dotnet/DocsExample/Program.cs?range=104-106)]

> [!NEXT]
> [Read this guide](~/dotnet-api/reading-events.md) for more information on how to read events with the .NET API.

### [JVM client](#tab/tabid-jvm-client)

To use the JVM client, use the following method passing the stream name, the start point in the stream, and whether to follow links to the event data:

[!code-java[getting-started-read](../../EventStore.Samples.Java/src/main/java/org/eventstore/sample/ReadEventExample.java?start=21&end=29)]

* * *

## Read a single event

The feed has a single item inside of it, the one you posted in [part 1](~/getting-started/index.md). You can see details of the event in the _Stream Browser_ tab in the Admin UI by selecting a stream to see its events, and then selecting an event. Or with cURL, issue a `GET` to the `alternate` URI value from the response above.

### [Request](#tab/tabid-8)

[!code-bash[getting-started-read-event-request](~/code-examples/getting-started/read-event.sh?start=1&end=1)]

> [!NOTE]
> You can also use `Accept: text/xml` if you prefer XML.

### [Response](#tab/tabid-9)

[!code-http[getting-started-read-event-response](~/code-examples/getting-started/read-event.sh?range=3-)]

* * *

### [.NET client API](#tab/tabid-dotnet-read-event)

To use the .NET API, use the following method passing the stream name, the event you want to read and wether to return the event data:

[!code-csharp[getting-started-read-stream-request](../../EventStore.Samples.Dotnet/DocsExample/Program.cs?start=108&end=109)]

### [JVM client](#tab/tabid-jvm-read-event)

To use the Java client, use the following method passing the stream name, the event you want to read and if you want to also return the event data:

[!code-java[getting-started-read](../../EventStore.Samples.Java/src/main/java/org/eventstore/sample/ReadEventExample.java?start=21&end=29)]

* * *

## Paginating through events

For longer feeds of events than this example, you need to paginate through the feed, reading a certain number of events at a time.

You can use the HTTP API [to paginate](~/http-api/reading-streams.md#feed-paging) through the feed using _previous_ and _next_ links within the stream. Or you can [use a read method](~/dotnet-api/reading-events.md#example-read-an-entire-stream-forwards-from-start-to-end) of the .NET API (and JVM client), to loop through events a certain number at a time.

## Subscribing to receive stream updates

A common operation is to subscribe to a stream and receive notifications for changes. As new events arrive, you continue following them.

You can create subscriptions and watch events as they arrive under the _Persistent Subscriptions_ tab, or use an API method:

![Subscriptions in the Admin UI](~/images/getting-started-subscriptions.png)

### [HTTP API](#tab/tabid-create-sub-http)

[!code-bash[getting-started-create-subscription](~/code-examples/getting-started/creating-subscription.sh?range=1-2)]

### [.NET API](#tab/tabid-create-sub-dotnet)

[!code-csharp[getting-started-read-stream-request](../../EventStore.Samples.Dotnet/DocsExample/Program.cs?range=119-133)]

> [!NEXT]
> Find more details on the parameters used in the example above, read the API documentation for [`PersistentSubscriptionSettings`](xref:EventStore.ClientAPI.PersistentSubscriptionSettings), [`CreatePersistentSubscriptionAsync`](xref:EventStore.ClientAPI.IEventStoreConnection.CreatePersistentSubscriptionAsync*) and [`ConnectToPersistentSubscription`](xref:EventStore.ClientAPI.IEventStoreConnection.ConnectToPersistentSubscriptionAsync*)

### [JVM client API](#tab/tabid-create-sub-java)

[!code-java[getting-started-read](../../EventStore.Samples.Java/src/main/java/org/eventstore/sample/SubscriptionExample.java?start=15&end=35)]

* * *

<!-- TODO: The dotnet example has reading events as they are added to subscription, HTTP does not, expand? -->

<!-- TODO: And write more events, then what etc -->

### Subscription types

There are three types of subscription patterns, useful in different situations

#### Volatile subscriptions

This subscription calls a given function for events written after establishing the subscription. They are useful when you need notification of new events with minimal latency, but where it's not necessary to process every event.

<!-- TODO: Example needed? -->

For example, if a stream has 100 events in it when a subscriber connects, the subscriber can expect to see event number 101 onwards until the time the subscription is closed or dropped.

#### Catch-up subscriptions

This subscription specifies a starting point, in the form of an event number or transaction file position. You call the function for events from the starting point until the end of the stream, and then for subsequently written events.

<!-- TODO: Example needed? -->

For example, if you specify a starting point of 50 when a stream has 100 events, the subscriber can expect to see events 51 through 100, and then any events are subsequently written until you drop or close the subscription.

#### Persistent subscriptions

> [!NOTE]
> Persistent subscriptions exist in version 3.2.0 and above of Event Store.

This subscription supports the "[competing consumers](https://docs.microsoft.com/en-us/azure/architecture/patterns/competing-consumers)" messaging pattern and are useful when you need to distribute messages to many workers. Event Store saves the subscription state server-side and allows for at-least-once delivery guarantees across multiple consumers on the same stream. It is possible to have many groups of consumers compete on the same stream, with each group getting an at-least-once guarantee.

<!-- TODO: Example needed? -->

## Next step

In this second part of our getting started guide you learned how to read events from a stream and subscribe to changes. The next part covers projections, used to give you continuous queries of your data.

-   [Step 3 - Projections](~/getting-started/projections.md)
