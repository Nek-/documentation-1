---
section: "Server"
version: "4.0.2"
---

# Metadata and Reserved Names

## Stream Metadata

Every stream in Event Store has metadata associated with it. Event Store allows you to change some values in the metadata, and you can write your own additional data into stream metadata (such as how often to snapshot for your own code).

### Reserved Names

All internal data used by Event Store will is prefixed with a `$` character (for example `$maxCount` on a stream’s metadata), because of this you should not use names with a `$` prefix as event names, metadata keys, or stream names, except where detailed below.

The supported internal settings are:

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>$maxAge</td>
            <td>Sets a sliding window based on dates. When data reaches a certain age it will disappear automatically from the stream and will be considered eligible for scavenging. This value is set as an integer representing the number of seconds. This value must be >=1.</td>
        </tr>
        <tr>
            <td>$maxCount</td>
            <td>Sets a sliding window based on the number of items in the stream. When data reaches a certain length it will disappear automatically from the stream and will be considered eligible for scavenging. This value is set as an integer representing the count of items. This value must be >= 1.</td>
        </tr>
        <tr>
            <td>$cacheControl</td>
            <td>This controls the cache of the head of a stream. Most URIs in a stream are infinitely cacheable but the head by default will not cache. It may be preferable in some situations to set a small amount of caching on the head to allow intermediaries to handle polls (say 10 seconds). The argument is an integer representing the seconds to cache. This value must be >= 1.</td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> If you set both `$maxAge` and `$maxCount` then events will become eligible for scavanging when either criteria is met. For example, if you set `$maxAge` to 10 and `$maxCount` to 50,000, events will be marked as eligible for scavenging after either 10 seconds, or 50,000 events, have passed. Deleted items will only actually be removed once the scavenge process is run.

Security access control lists are also included in the `$acl` section of the stream metadata.

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>$r</td>
            <td>The list of users with read permissions</td>
        </tr>
        <tr>
            <td>$w</td>
            <td>The list of users with write permissions</td>
        </tr>
        <tr>
            <td>$d</td>
            <td>The list of users with delete permissions</td>
        </tr>
        <tr>
            <td>$mw</td>
            <td>The list of users with write permissions to stream metadata</td>
        </tr>
        <tr>
            <td>$mr</td>
            <td>The list of users with read permissions to stream metadata</td>
        </tr>
    </tbody>
</table>

You can find more details on access control lists can [here](access-control-lists.md).

## Event Metadata

<!--  TODO: Is this duplicated or different?-->

Every event in Event Store can have metadata associated with it. Event Store supports some values being set in the metadata and you can write your own additional data into event metadata if you wish (such as the source of the event). All names starting with `$` are however a reserved space for internal details. The currently supported internal details are:

-   `$correlationId` The application level correlation ID associated with this message.
-   `$causationId` The application level causation ID associated with this message.

Projections will honor both the `correlationId` and `causationId` patterns for any events it produces internally (linkTo/emit/etc).
